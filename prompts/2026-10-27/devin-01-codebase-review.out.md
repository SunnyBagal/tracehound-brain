TraceHound audit: read-only. No commits, no model/API calls, blind sets not opened.

Coverage gap first: I read src/review/, src/llm/, packages/action (pipeline, action, reviewer), the composite action and the review workflow line by line. I only listed the files in src/contracts/, viewer/, CI/image workflows, and most of scripts/bench/, so I have no claims about them. Your backlog's "merge-base fetch duplicated in viewer/lib/data.ts": no such file exists. The merge-base/fetch code is in data.ts:198-204.

Bugs (worst first)
High: truncation is retried into the same wall. review.ts:178-196 never reads finishReason, which the client does expose (client.ts:368-375).
What breaks: a reply cut off at 32k counts as "not valid JSON". The retry appends the whole cut-off reply (L181-183), keeps the same max_tokens and reasoning on (L155-161), so it has more input under the same cap. review-ctx.ts does the same (L70-71).
Repro: give an offline stub client a cut-off reply with finishReason:"length". The chunk ends CHUNK_FAILED after 2 calls.
Fix: when finishReason==="length", retry with the original messages only, reasoning off or a reasoning budget.
Unverified: how many of the ~7 failed PRs per run this causes.
Medium (bench/dev): the cache stores bad replies, so cached reruns can't recover. client.ts:296-297 caches any non-empty content. The key is sha256(model+request) (cache.ts:13-15) and requests are fixed at temperature 0.
What breaks: rerunning a failed PR replays the same bad first reply, then the same bad retry (its request contains that reply), and fails again.
Repro: rerun one failed dev PR with the cache in place; failure text is the same, spend is $0.
Fix: don't cache replies that fail to parse or end with finish_reason=length.
CI runners start with an empty cache, so the product is not affected.
Medium: the parser is strict about fences. review.ts:83-87: the fence regex is anchored ^…$, so any prose before or after the fence, or a stray closing brace, fails the chunk. Same strictness in parseVerdictReply (verify-ctx.ts:70).
Repro: parseReviewReply('Here:\n```json\n{"findings":[]}\n```') returns ok:false.
Fix: take the last balanced {…} that contains "findings".
Medium (risk): the job timeout is far below worst-case model time. tracehound-review.yml:19-19 sets 20 minutes. Each call may take 600 s (cli.ts:61-61), chunks run one after another (review.ts:150), with up to 2 attempts each, then verify and fixes.
What breaks: a 3-chunk PR can take more than 60 minutes in the worst case. GitHub kills the job with no "nothing posted" log line.
Fix: a per-stage time limit below the job timeout.
Unverified that this happens in practice; the measured maximum on 3.5 dev was about 13 minutes.
Medium: findings with no line are never deduplicated. agree-rule.ts:7-8 requires both lines non-null, and pipeline.ts:64-72 accepts line:null.
What breaks: the same file-level finding from both reviewers enters the pool twice. The contracts dedup (action.ts:279-280) has the same hole.
Repro: poolFindings([{path:"a",line:null,body:"x"}],[{path:"a",line:null,body:"x"}]).length === 2.
Fix: also treat same path with both lines null as agreeing.
Low-Medium: the spend cap is per process, not per PR. Each stage runs as its own CLI process (pipeline.ts:104-107). The diff and ctx reviewers run at the same time (L123-126). Each builds its own Budget, with spentBeforeRun read once (budget.ts:30-46).
What breaks: the effective per-PR cap is about 3×$1 plus fixes. Two processes near the total cap can both pass the check.
Repro: set TRACEHOUND_BUDGET_TOTAL_USD just above the ledger total and run the pipeline; both reviewers spend.
Fix: pass each stage its own run cap from the parent, or lock the ledger.
Low: review-diff-v1 is still all-or-nothing. action.ts:273-276 posts nothing if any chunk failed. Decision 068 changed this only for pool-verify-v1. Fix: the same partial-chunk rule plus a body note.
Code improvements
review.ts:139-200 and review-ctx.ts:52-115 are copies of the same review/retry/validate loop. Bugs 1 and 3 have to be fixed twice and the paths will drift. M.
pipeline.ts:6: product code imports scripts/bench/agree-rule.ts. Move the rule into the analyzer and have the bench import it from there. S.
review.ts:44-45: ChunkRecord stores no finishReason, raw reply or reasoning-token count, so failures can't be split into truncation vs malformed output without the cache. S.
action.ts:163 caps in pool order (diff findings first, ctx-only last). With max-comments: 10 the ctx-only findings are cut first whatever their merit. sources (pipeline.ts:76-81) already marks "both"; sort on it before the cap. S.
verify-ctx.ts:90: VERIFY_FAILED findings are kept and posted like verified ones. Keeping them is a reasonable recall choice, but mark them in the comment, not just in the footer count. S.
diff.ts:132: a single hunk over 60k is sent as is (overLimit:true), which is the likeliest input to hit the 32k output cap. Split it on line boundaries. M.
review.ts:15: REASONING_ON is enable_thinking only, with no reasoning budget. NVIDIA documents chat_template_kwargs.reasoning_budget (whether Token Factory honours it for 3.5 is unverified). S.
Temperature is hard-coded to 0 in review.ts:155, review-ctx.ts:70, verify-ctx.ts:121 and fix.ts:116. Make it an option (recorded with the prompt SHA) so sampling experiments don't need code forks. S.
client.ts ChatRequest has no response_format field, so schema-constrained output can't be tried without changing the client type. S.
pipeline.ts:181: the failure log has no stage counts. Already on your backlog; noted only because it pairs with improvement 3. S.
Ideas to raise F (ranked by expected gain ÷ cost)
Truncation-aware salvage of saved replies.
What: a lenient parser (bug 3) plus recovery of complete findings[] items from cut-off replies.
Source: Nebius structured output.
Plugs in: review.ts:81-94, review-ctx.ts. Targets: recall.
Test: $0 replay over the cached raw bodies in .tracehound/cache from the 3.5 dev runs (setup.ts:25; unverified that they're still on disk), re-scored with the existing scorer.
Win: same records compared pair by pair, so run-to-run spread cancels. More true positives on the failed-chunk PRs, and precision of the salvaged findings at or above the run's precision.
Could fool us: findings from cut-off replies are the weakest; score them separately and with both judges.
Reasoning budget plus response_format: json_schema.
Source: NVIDIA reasoning budget, thinking budget control, Nebius docs above (model support varies).
Plugs in: review.ts:15/154-161, client.ts ChatRequest. Targets: recall and cost.
Test: a 2-PR smoke check (cents) that the 3.5 endpoint accepts the schema, then one dev run at about the cost of a 3.5 dev run (see dev-3.5.md).
Win: at most 1 of 40 PRs with failed chunks, about 0 calls at the 32k cap, and F not below the 3.5 two-run band.
Could fool us: the schema or budget makes reasoning shallower, so failures fall while recall on hard PRs falls too. Judge it on F, not on failure count.
Temperature 1.0 with diff-order shuffling, scored by agreement (self-consistency).
Source: Cursor Bugbot, self-consistency, SWR-Bench Multi-Review.
Why it differs from 064 and the levers-v2 2-of-3 vote: those voted over temperature-0 runs, whose differences are serving noise, not sampled reasoning paths. This uses NVIDIA's recommended sampling plus a shuffled diff order to make passes independent, and uses agreement to rank, not as a hard filter.
Plugs in: pipeline.ts:123-135, agree-rule.ts. Targets: precision (vote) or recall (union).
Test: dev run with k=3 at about 3× reviewer cost.
Win: mean F over at least 3 seeds above the 3.5 mean by more than one run-to-run SD, under both judges.
Could fool us: the union inflates volume, which the metric rewards; require precision not to drop.
Verifier as a two-sample drop. Drop a finding only if 2 of 2 verifier samples say drop.
Source: BitsAI-CR ReviewFilter.
Why it's new: levers-v2 showed the 3.5 raw pool beats verified output, so the verifier's single-sample drops are where F is lost.
Plugs in: verify-ctx.ts:118-170. Targets: recall, while keeping precision.
Test: verifier-only rerun over saved dev pools (only verifier calls, a few dollars).
Win: F of the verified output at least the raw-pool F on the same pools.
Could fool us: if it converges to "keep everything", it's 056 again. Report the drop rate.
Context from the component graph instead of lexical git grep. Reserve budget for callers and definitions (context.ts:25/64, verify-context.ts:28; references survived in only 12 of 48 chunks per 057).
Source: Greptile (full-codebase context).
Targets: recall.
Test: $0 review-ctx --context-only (ctx-cli.ts:26) to measure how many references survive the budget, then one dev run.
Could fool us: ts-morph covers TypeScript only, while dev is multi-language (names.ts). Don't tune to the worst repo.
Fixed category filter for non-defects (style, documentation, compiler warnings).
Source: Bugbot step 5, BitsAI-CR taxonomy. Plugs in: after verify, pipeline.ts:140. Targets: precision.
Test: needs a severity/category field on findings; a dev run.
Could fool us: Greptile found LLM severity scores were close to random, so use a fixed category list, not a 1-10 score.
Learning from addressed or ignored comments with embeddings.
Source: Greptile nitpicks. Plugs in: action learn.ts.
Test: needs new data (real PR feedback); can't be benchmarked on dev without leaking the answer key.
Recommendation
Run idea 1 first, as a $0 paired replay over the cached 3.5 dev replies:

It is the only test here that is immune to the 35.6–48.0 spread, because it uses the same records.
It costs nothing.
It targets the largest measured loss (about 7 of 40 PRs with dead chunks).
Its result tells you whether idea 2 is worth paying for.
First check that .tracehound/cache still holds those bodies (unverified).

Sources
https://cursor.com/blog/building-bugbot
https://www.greptile.com/blog/make-llms-shut-up
https://www.greptile.com/docs/how-greptile-works/nitpicks
https://arxiv.org/html/2501.15134
https://arxiv.org/html/2509.01494
https://arxiv.org/abs/2203.11171
https://docs.tokenfactory.nebius.com/ai-models-inference/json
https://opencolin.github.io/nebius-ecosystem-cookbook/cookbook/r/guided_json.html
https://docs.api.nvidia.com/nim/re/reference/nvidia-nemotron-3-super-120b-a12b
https://docs.nvidia.com/nim/large-language-models/latest/thinking-budget-control.html