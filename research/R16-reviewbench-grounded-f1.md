# TraceHound on ReviewBench: what would actually raise grounded F1 in two days

The biggest and cheapest gain for TraceHound is more grounded recall per PR. Stop throwing findings away and stop truncating chunks, then switch the generator to a per-file, multi-lens "report every real issue, including low-severity ones" mode. The leaderboard's own A/B evidence shows the tooling barely matters: turning off a confidence gate, or turning on nits, is worth far more than adding Semgrep, a verifier or Nemotron 3 Ultra.

## TL;DR

- **ReviewBench is real and your description of it is largely right, with some corrections.** It is GitHub's benchmark. The GitHub Blog launch post (5 Oct 2026) says it "contains 219 pull requests from 187 public open source licensed repositories spanning 19 languages". Matching is per file and semantic, and grounded precision ignores unmatched findings. Your leaderboard numbers match the 19 Oct mirror exactly. One correction: inferred author actions and Semgrep contribute almost nothing to the golden set's true positives. About 98% of them come from LLM reviewers.
- **The top entries win on recall, and the clearest evidence for what drives recall is on the leaderboard itself.** Klaussy's default-to-strict ("include nits") switch took recall from 22.6% to 29.8%. block/review's leaderboard run had its confidence gate off. RevEngine Prime uses a fixed list of lenses, including tests and docs, plus a "look once more" pass. TraceHound gets about 1.3 golden TPs per PR from about 5 findings. DeepSeek Harness gets about 3 from about 4.5. Per-finding yield matters as much as volume.
- **A recall-mode v3 fits the budget on Nemotron 3 Super.** Token Factory's model catalog lists it at $0.30/$0.90 per million tokens. Other providers list lower output prices, for example Baseten at $0.30/$0.75 and DeepInfra at $0.10/$0.50. The plan runs per-file chunks × 2 lenses plus one missed-issue pass, with no Ultra and no verifier, at an estimated ~$0.05 per PR, or ~$33 for 219 × 3. It finishes well inside 1800 s. Expect grounded recall to rise from 9% to roughly 18–25% and grounded F1 to roughly 28–37. That range is my estimate, not a measurement. The real risk is augmented precision, which is already 52.5 against 80–92 for every published entry.

## Verification of ReviewBench and your description

**Existence and identity: confirmed.** ReviewBench is GitHub's benchmark, announced on the GitHub Blog on 5 October 2026 by Michelle Zhou and Alejandro Carderera de Diego.\[1\]\[2\] The repo is github.com/review-bench/ReviewBench under the MIT licence, and the site is review-bench.ai.\[3\]\[4\] I could not open review-bench.ai itself. Leaderboard numbers below come from the BenchLM mirror of the official leaderboard (19 Oct 2026 capture),\[5\] cross-checked against a 6 Oct read of `/api/leaderboard` quoted in JPHutchins/code-review issue #282 and a Daydream issue that re-read the official board.\[5\]\[6\]

**Your scoring description, item by item:**

| Your claim | Status | Detail |
|---|---|---|
| ~4,600 findings, 219 PRs, ~12 TPs/PR | Consistent | A third-party tally (JPHutchins #282, which read `golden/`) reports 4,632 findings, 2,623 labelled TP, i.e. ~12.0 TP/PR.\[1\] I did not open golden data. The README says the 25-PR test set has 352 TP findings, i.e. ~14 TP/PR.\[4\] |
| Sources: human comments, inferred author actions, Semgrep, several LLMs | Confirmed in METHODOLOGY.md, but misleading in practice | The same third-party tally attributes TPs as: `claude-sonnet-4.6` 46.0%, CCR (gpt-5.5) 36.5%, CCR (claude-opus-4.7) 8.7%, gemini-2.5-pro 5.7%, human_review 1.5%, gpt-4o 1.4%, Semgrep 0.3% (8 TPs), inferred_author_action 0%.\[1\] Deduplication keeps one surviving finding per issue, so inferred actions may be hidden inside LLM findings.\[7\] |
| Sonnet classifier labelled TP/FP, humans corrected | Confirmed | Initially Claude Sonnet 4.6, later updated to Sonnet 5.\[7\] Humans audited the labels and corrected 47 findings wrongly labelled TP. heise reports that independent senior engineers classified findings the same way as ReviewBench "in 96.6 percent of cases", and that ReviewBench "uses Claude Sonnet 5" as its evaluation model. |
| LLM matcher, same file only, "same underlying issue" | Confirmed | "Findings in different files are never compared." Matching is semantic, "even if their messages, line ranges, or framing differ". The matcher reasons about "what a minimal fix would look like". It sees only file path, line range and message,\[7\] and runs with no tools.\[1\]\[7\] |
| Grounded P = matched-to-golden-TP / all matched; unmatched excluded; grounded R = covered golden TPs / all golden TPs | Confirmed, with one nuance | Recall uses raw many-to-many matches, so one candidate can cover several golden findings.\[7\] For precision, each candidate claims at most one unclaimed golden finding. A duplicate whose matches are all claimed becomes "unmatched" and goes to the classifier, where it counts only in augmented metrics.\[7\] |
| Leaderboard precision 84–90% for everyone | Confirmed (83.6–90.2) | Across all 33 published configurations.\[5\] |
| Augmented metrics penalise unmatched FPs | Confirmed | Augmented P = (M_TP + U_TP)/|C|. Augmented recall's denominator differs per agent, and GitHub calls it a per-system diagnostic.\[2\]\[5\]\[7\] |
| Final = 219 PRs × 3 rounds, mean of the three | Confirmed | Scores publish only on a first entry or when they beat your previous row. Your row shows how many configurations you tested.\[2\]\[4\] |
| 1800 s per PR | Consistent, not opened directly | The contract at commit aee9318 said "15 minutes per pull request by default", with `pr_timeout_seconds` up to 3600.\[1\] A later ReviewBench commit is titled "correct its default to 1800", and Klaussy notes the limit "is now 30 minutes".\[8\]\[9\] |

**Things I could not open (UNVERIFIED):** `AGENT_CONTRACT.md`, `scripts/eval/matcher.ts` (the matcher prompt text), `scripts/classifier/prompts.ts`, `docs/EXTRACTION.md` and the manifest schema. My research subagent could not open them either. Where I cite them, I am relying on verbatim quotes in JPHutchins #282 (pinned at commit aee9318, which may be stale) and on a third-party adapter README. The Semgrep rule sets used are unknown. The dataset `ai-code-review-evaluations/golden_comments` was not checked.

**Leaderboard: your numbers verified, plus entries you are missing.** All 11 of your figures match the 19 Oct capture exactly. Labels you should know:

- "Klaussy 44.1" is the strict, include-nits configuration.\[5\] Klaussy's default configuration is a separate row at 35.9.\[10\]
- "Copilot 40.1" is Balanced.\[11\] Copilot Lite is 28.6.\[5\]
- "Codex 30.4" is GPT-5.6 Sol at max reasoning.\[5\]

There are 33 configurations in total,\[5\] mostly Codex model and effort variants from 10.6 to 30.4.\[5\] Several more agents have been onboarded but had no published score as of that capture: diffox, diffox ultra, emirtuncer-claude, intent-review-agent, OhMyBug, alma-reviewer and opswiki.\[8\] Perch's README claims "81.1%" ReviewBench recall.\[12\] That number is self-reported, does not appear on the leaderboard, and is unverified.

## 1. What the top entries do

| Entry (grounded F1 / P / R) | Model | Passes and structure | Lenses | Tools and exploration | Sampling | Verifier or filter | Findings/PR |
|---|---|---|---|---|---|---|---|
| Laude RevEngine Prime (59.6 / 83.6 / 46.3) | Claude Opus 5.5 | One or more models review independently, then are "asked to look once more" | One reviewer checks a fixed list: design fit, unexpected additions, correctness, production safety, security, data exposure, performance, compatibility, tests, documentation | Read-only: diff, files at head and base, listings, text search. Never runs code | "One or more models" | Verification pass rejects findings that don't hold; deterministic reduction checks path and line range against head, merges duplicates, ranks and caps | Not published |
| block/review (47.7 / 88.7 / 32.6) | gpt-6.1-sol + claude-opus-5-5 ("sol-opus") | Panel → coordinator → judge → delivery | 3 lenses (Behavior & Contracts; Reliability & Operations; Security & Trust Boundaries), each run per model, so 6 panelists | Goose agent with the developer extension (shell) | Two models per lens | Coordinator drops a finding only with concrete counter-evidence; judge restores wrong drops; delivery gate (P0–P2, confidence ≥ 0.8) was **off** for the leaderboard run | Not published |
| Klaussy strict (44.1 / 84.1 / 29.8) | Claude Sonnet 5.5 via Claude Code | Lens sub-agents plus synthesis | Correctness and scope lenses at final time; docs, tests and accessibility lenses added *after* the final | Claude Code tools; PR title and body passed in | Not stated | Validator; strict mode also prints Warn and Nit | Not published; ~$3.2 per PR on one test PR |
| Copilot code review Balanced (40.1 / 87.8 / 26.0) | Undisclosed | Agentic tool calling (GA March 2026); a lite-tier multi-model ensemble was tested | Not disclosed | Tool calling for repo context; deterministic ESLint and CodeQL integration | Ensemble in lite tier | Not disclosed | ~5.1 per review (third-party blog, weak source) |
| DeepSeek Harness Reviewer (39.9 / 89.5 / 25.6) | deepseek-flash, effort high, 2400 s timeout | DeepSeek's dsh agent harness | UNVERIFIED | Agent harness; egress only to api.deepseek.com | UNVERIFIED | UNVERIFIED | ~4.5 (978 findings in one 219-PR round) |
| Devin AI (37.0 / 84.0 / 23.8) | Undisclosed | "Bug catching agent" | Not disclosed | Scans the broader codebase beyond the diff | Not disclosed | Labels by confidence and severity | Not published |
| Greptile (27.2 / 86.1 / 16.2) | Undisclosed (third parties say v3 uses the Claude Agent SDK) | Code-graph index plus multi-hop investigation | Not disclosed | Repo indexing | Not disclosed | Not disclosed | Not published |
| Cursor (17.3 / 87.7 / 9.6) | Undisclosed | Cursor's Bugbot blog, quoted on the Cursor forum, describes "eight parallel passes with randomized diff order". Third-party site aicodereview.cc says Cursor rebuilt it "to a fully agentic design" in fall 2025 | Logic bugs | Agentic, per third-party reports | 8 passes | Per the same blog: "Majority voting to filter out bugs found during only one pass", then a validator model, and it filters "unwanted categories (like compiler warnings or documentation errors)" | ~0.7 bugs per PR (Cursor-reported, via third party) |

**Notes that matter:**

- **block/review is open source** (Apache-2.0, Go, built on Goose).\[13\] I could read only its README, not the prompt files. Its prompt text is UNVERIFIED.
  - The README describes the pipeline. Panelists "each review the whole change through one fixed lens" and report "evidence, priority (P0–P3), and an honest confidence".\[13\]
  - The coordinator "merges duplicates and tries to disprove every candidate", dropping only "with concrete counter-evidence".\[13\]
  - The judge "re-examines only the dropped candidates", and its docs recommend a different model from the coordinator.\[13\]
  - The time budget is up to 7 minutes for the panel, with the coordinator and judge sharing the rest of 14 minutes.\[13\]
  - The key fact for you: the leaderboard row is `gate=off`.\[5\] The best-scoring config ships every finding the coordinator did not disprove.
- **RevEngine is also open** (laude-institute/rev-engine and rev-engine-prime, Apache-2.0).\[14\]\[15\] Its mean duration is about 3 minutes, and a typical review costs about $1 in OpenRouter tokens with Opus 5.5.\[6\]\[14\] The rev-engine-prime README still says the agent "has not been registered with the benchmark and has no published score", which contradicts the leaderboard.\[15\] Treat the README as stale.
- **The Cursor row shows how precision-first design kills recall.** Majority voting plus dropping documentation findings puts it at 9.6% recall, the same as TraceHound.\[5\]\[16\]
- **The Devin, Qodo, Greptile, Cursor and Cubic rows are GitHub-App products.** The public docs do not say how they were run, and the repo has no manifests for them.\[1\]

## 2. ReviewBench specifics that matter for recall

**Agent contract (from secondhand quotes; the file itself is UNVERIFIED).**

- **Output** is a JSON file at `$RB_OUT` with `pr`, `agent` and `findings[]`. Each finding has exactly `{file, start_line, end_line, message, producer}`.\[17\]\[18\]
- **Severity and category** are assigned by ReviewBench's classifier, not by the agent.\[18\]
- **Inputs:** a writable `/work/repo` at head with a shallow `.git`, `/work/pr/diff.patch`, and `/work/pr/pr.json` (title and body included).\[1\]\[19\]\[20\]
- **Restrictions:** no GitHub API; egress only to hosts declared in the manifest, port 443, at most 32 hosts.\[1\]\[21\]
- **Failure rule:** "One pull request that never succeeds fails the whole run".\[1\]
- **No contract-level cap** on the number of findings turned up in any source I could read. Your own adapter (TraceHound PR #50) caps output at 40, writes one line as both `start_line` and `end_line`, and drops null-line findings.\[19\] All three are self-imposed choices.

**The matcher: what makes two findings "the same issue".** METHODOLOGY.md says matching is semantic: "two findings match if they describe the same underlying issue, even if their messages, line ranges, or framing differ", while two findings at the same location "do not match if they describe different issues". The matcher reasons about the minimal fix.\[7\] Implications:

- **The file is the only hard constraint.** Line precision is not required. A finding in the wrong file can never match. A finding with no line is dropped by your adapter and so can never match.
- **The message must name the defect and the fix.** The matcher sees only the message text, path and range, and has no tools to look anything up.\[1\]\[7\]
- **Many-to-many matching means one broad finding can cover several golden findings for recall.**\[7\] Exploiting this with kitchen-sink messages is gaming. The labelling rules also split multi-category findings, so keep one issue per finding.\[7\]

**Caps and duplicate penalties.**

- **Grounded metrics:** no duplicate penalty. A duplicate that cannot claim an unclaimed golden finding becomes unmatched, and unmatched findings are excluded from grounded P and R.\[7\]
- **Augmented metrics:** duplicates go to the classifier, and "duplicates" is a listed FP mode.\[7\] Deduplication therefore protects augmented precision, not grounded F1.

**How inferred author actions and tool findings were produced.**

- **Inferred author actions:** a finding is inferred when a follow-up commit "plausibly responds to a review observation". Inference is "conservative" and anchored to the modified lines.\[7\]
- **Deterministic tools:** "run against the PR's diff or the changed files". A ReviewBench doc snippet says "currently Semgrep only".\[7\]
- **Which Semgrep rule sets: UNVERIFIED.** The third-party tally gives Semgrep 146 findings and 8 TPs, a 5.5% TP rate.\[7\]

**The classifier's FP guidelines (avoid generating these).** METHODOLOGY.md lists the failure modes as "incorrect claims, unverifiable claims, off-scope observations whose scope is judged out of bounds for the PR, duplicates, sub-threshold style nits, and advice that would degrade the change".\[7\]

- **Unverifiable from reading means FP.** The classifier can only read files and cannot run a shell, so a claim it cannot confirm by reading the code is labelled FP.\[1\] Every finding should therefore quote the code it relies on.
- **Scope is not a criterion.** Findings in pre-existing code or untouched files can be TPs if they are correct, relevant and non-trivial.\[7\]
- **Tests and docs count.** Testing (35) and Documentation (21) are 16% of the test set's 352 TP findings, and 51.7% of test-set TPs are low severity.\[4\]

## 3. Evidence-backed levers, ranked

The baseline is your 25-PR test run: R 9.0, P 70.6, about 5 findings per PR. At ~14 TPs per test-set PR, that is about 1.3 golden TPs covered per PR. DeepSeek Harness covers about 3 per PR (25.6% of ~12) with about 4.5 findings.\[5\]\[22\] The gap is both volume and yield per finding.

The Δ column is my estimate in absolute grounded-recall points from your 9.0 baseline. **None of these numbers has been measured on Nemotron.** Treat them as priors to test on your dev set. Gains overlap and will not simply add up.

| Rank | Lever | Expected Δ grounded recall | Cost × | Days | Evidence |
|---|---|---|---|---|---|
| 1 | Stop losing findings: anchor null-line findings to the first changed line of the file, use hunk-span line ranges, lift the 40-finding cap to ~60 | +0.5–2 | 1.0 | 0.25 | TraceHound PR #50 adapter behaviour;\[19\] METHODOLOGY §6.2 (file is the only hard constraint)\[7\] |
| 2 | Fix truncation: set a reasoning budget or low-effort mode, use max_tokens ≥ 16k, retry on `finish_reason=length` with reasoning off or a smaller chunk | +1.5–3 (recovers ~1/6 of chunks) | 1.1–1.3 | 0.25 | Your own 1-in-6 loss; NVIDIA model card (`reasoning_budget`, low-effort mode); Codex ladder on ReviewBench, GPT-5.6 Sol low → high recall 9.7 → 15.8 |
| 3 | Recall-mode prompt: "report every real issue, including low severity and nits, no quota, no invention" | +3–6 | 1.2 | 0.25 | Klaussy default → strict: R 22.6 → 29.8, augmented P 88.1 → 81.0; block/review leaderboard run gate=off; 51.7% of test-set TPs are low severity |
| 4 | Per-file fan-out with chunking (≤ ~8k diff tokens per call) | +3–6 | 2–3 (input) | 0.5 | Klaussy's chunking commit ("on a big PR every reviewer worked from the first files and missed the rest"); c-CRAB F1 falls from 0.657 (<10-line diffs) to 0.043 (>150 lines); 35.6% of ReviewBench PRs exceed 1,000 lines\[4\] |
| 5 | Two or three diverse lenses, unioned (correctness/reliability/security; tests/docs/API/maintainability/completeness) | +2–5 | ×(number of lenses) on output | 0.5 | block/review 3 lenses; RevEngine Prime's 10-item lens list; Klaussy added docs, tests and accessibility lenses after finding its weakest categories were testing (22%) and docs (27%) |
| 6 | Missed-issue pass: "here are the findings so far; what real issues are missing?" | +1–3 | 1.2–1.3 | 0.25 | RevEngine Prime is "asked to look once more"; CR-Bench Reflexion raised GPT-5.2 recall 27.01% → 32.76% while SNR fell 5.11 → 1.95 |
| 7 | Message format for the matcher: "<symbol>: <defect> → <consequence>. Fix: <minimal change>." quoting the line | +0.5–2 (yield, not volume) | 1.0 | 0.1 | Matcher reasons about the "minimal fix" and sees only path, range and message; "Unverifiable" is an FP mode\[1\]\[7\] |
| 8 | Sampling union, 2 samples at T=1.0, top_p=0.95 | +1–3 | ×2 | 0.25 | NVIDIA recommends T=1.0 / top_p=0.95 "across all tasks". GitHub's multi-model CCR ensemble: Help Net Security (6 Oct 2026) reports "recall rose by 13.6%" relative to the production control, with a +12.88% offline prediction. BugBot ran 8 passes but voted for precision |
| 9 | Dedup (same file, overlapping lines, similar text) | ~0 | 1.0 | 0.1 | Grounded metrics ignore unmatched duplicates; only augmented P benefits\[7\] |
| 10 | Nemotron 3 Ultra instead of Super | Unknown, plausibly +1–3 | 3.3 | 0.1 | Model strength matters on this board (Codex Sol max 18.5 vs Terra max 14.5 recall), but Ultra at $1/$3 per million tokens breaks the budget |
| 11 | Static analysers as a finding source (Semgrep, ESLint, ruff, tsc, gitleaks) | 0–0.5 | 1.0 (CPU) | 0.5–1 | Semgrep has 8 golden TPs (0.3%) at a 5.5% TP rate per third-party tally;\[7\] offline rules and per-repo configs are fragile. If used at all, feed results to the LLM as hints only |
| 12 | "What would the author fix next" prompts | ~0 as a separate pass | 1.2 | 0.25 | Inferred author actions have 0 surviving golden TPs per third-party tally;\[1\] fold a "completeness" question into lens 5 instead |
| — | Verifier or filter | Negative for recall | 1.2–1.5 | 0.25 | Leaderboard leaders use verifiers that drop only on counter-evidence; skip a verifier in recall mode, or use disprove-only |

**Lever notes:**

- **Sampling at temperature 1.0 is NVIDIA's recommendation, not a tuning choice.** Running below 1.0 is off-spec for Nemotron 3 Super and 3.5 Super VL.\[23\]\[24\] The recall gain comes from the union across samples, but each extra sample costs ×2. Spend the money on lenses and chunks first.
- **The reasoning budget is your cheapest recall lever.** NVIDIA's `reasoning_budget` closes the reasoning trace at the next newline once the budget is hit, or "abruptly at reasoning_budget + 500" if none comes.\[23\] A model-catalog listing shows a 32,768-token max output for Super on Token Factory.\[25\] A ~2–4k reasoning budget inside a 16k max_tokens should end most truncations.
- **The "tests and docs" lens is the most clearly under-served area.** One caveat: Klaussy's category percentages come from a gap analysis Klaussy ran against the golden set.\[9\] They are aggregate, public and not golden contents, but if your pre-registration forbids even golden-derived aggregates, rely on the README's category table instead.

## 4. Two-day plan: recall-mode v3

**Architecture (all Nemotron 3 Super, T=1.0, top_p=0.95):**

1. **Prep (deterministic).**
   - Parse `diff.patch`.
   - Drop lockfiles, generated, minified and binary files.
   - Pack the diff into chunks of ≤ ~8k tokens, splitting big files at hunk boundaries and keeping line numbers.
   - Cap a PR at 8 chunks. Prioritise source over tests over config over docs, and log what was skipped.
2. **Lens A, one call per chunk:** correctness, reliability (error handling, concurrency, resource leaks), security, performance, API and compatibility.
3. **Lens B, one call per chunk:** tests (missing or ineffective tests, naming the regression they would let through), documentation and comments made stale by the change, typos in user-facing text, maintainability, and completeness ("does the change do everything it claims, everywhere it needs to?").
4. **Missed-issue pass, one call per PR.** Input: the PR title and body, a diff capped at ~25k tokens, and the deduplicated findings list. Ask: "list real issues not already covered".
5. **Reduce (deterministic).**
   - Validate that each path exists at head and that line ranges fall inside the file.
   - Anchor null lines to the first changed line in that file.
   - Merge duplicates (same file, overlapping lines, high text similarity).
   - Cap at 60 findings.
   - Write `[]` on any error, as you already do.
6. **Optional, if budget allows:** a disprove-only filter that drops a finding only when the model quotes code proving it false, as block/review's coordinator does. Run it as an A/B on the dev set.

**Prompt outline for lenses A and B:**

- Role: "senior reviewer on this repo".
- The PR title and body, fenced as untrusted context.
- A file list for the whole PR.
- The chunk with head line numbers.
- Instructions:
  - Report **every** real, verifiable issue, including low severity.
  - No quota; never invent.
  - One issue per finding.
  - Quote the line you rely on.
  - Anchor to the file where the fix belongs.
- Output JSON: `{file, start_line, end_line, message}`. Message format: "`<symbol>`: <what is wrong> → <consequence>. Fix: <minimal change>."
- Reasoning: low-effort or `reasoning_budget` ≈ 2–3k, max_tokens 16k.

**Cost per PR (estimate).** I assume ~9k input and ~2.5k output tokens per call at Token Factory's catalog rate of $0.30/$0.90 per million, which is about $0.005 per call. Other providers list cheaper output, for example Baseten at $0.75 and DeepInfra at $0.10/$0.50, so $0.90 is the conservative figure.

- Token Factory lists a cached-input rate of $0.06 for Nemotron 3.5 Super VL.\[26\] Shared prefixes would cut input cost further. I could not confirm a cached rate for 3 Super.
- Per-PR cost by size, using the full set's size distribution:

| PR size | Calls | Est. cost |
|---|---|---|
| ≤ 200 lines | 3 | ~$0.015–0.03 |
| ~500 lines | ~7 | ~$0.045 |
| > 1,000 lines | ~17 | ~$0.09 |

- **Weighted mean ≈ $0.05 per PR, or ≈ $33 for 657 runs.** That is inside $20–40 but in the upper half.
- **Enforce a hard per-PR token cap of about $0.10.**
- **Measure before you trust these numbers.** Run the 40-PR dev set first and use measured tokens, because reasoning-token use on Nemotron dominates output cost.
- **Ultra** at $1/$3 per million\[27\]\[28\] would put the same plan at roughly $110, so keep it out of the final.
- **A second sample** across all chunks would roughly double cost. Add it only to PRs under ~500 lines if the dev-set cost leaves room.

**Time.** Token Factory shows ~282 tok/s for 3.5 Super VL.\[26\] At that speed a 2.5k-token output takes ~10 s. With 6–8 calls in parallel, even a 17-call PR should finish in a few minutes, far inside 1800 s and inside your adapter's own 25-minute guard.\[19\] Token Factory rate limits are UNVERIFIED, so add backoff.

**Day plan:**

- **Day 1 AM:**
  - Levers 1, 2 and 7: anchoring, ranges, cap, truncation handling, message format.
  - Re-run v2 on the dev set as the baseline.
- **Day 1 PM:**
  - Chunking, lenses A and B, recall-mode prompts.
  - Dev-set run measuring recall, precision, cost and wall time.
- **Day 2 AM:**
  - Add the missed-issue pass.
  - Ablate each big lever on and off on the dev set, at least lenses, the missed-issue pass and the disprove filter.
- **Day 2 PM:**
  - Freeze and pre-register the config.
  - Run `scripts/try-agent.sh --set full` locally, which checks completion and format only, with no judge and no golden data.\[4\]
  - Run one portal test run and read only the aggregate number.
  - Then start the final.

**Validating without ReviewBench's golden set.**

- **Build your own matcher, scoped to your dev set.** Apply ReviewBench's published matching semantics (same file, same underlying issue, minimal-fix reasoning) to your 40-PR dev set. Use a judge model you do not generate with.
- **Report four numbers:**
  - Recall against human comments.
  - Matched precision.
  - An augmented-precision proxy: hand-audit or LLM-judge ~50 unmatched findings against "true, relevant, non-trivial, verifiable by reading".
  - Cost and seconds per PR.
- **Expect the dev set to under-predict.** Human comments are only ~1.5% of ReviewBench's golden TPs.\[4\] Your dev set therefore measures something closer to human-review recall, and will under-predict gains from low-severity and LLM-style findings. Use it to rank variants, not to forecast the leaderboard number.
- **The 25-PR portal test is a smoke test only.** The README says "Test set scores are not leaderboard scores".\[4\] With 25 PRs and one run, differences under several points are noise. The published ± values are 0.1–1.6 points, and those are already averages over 219 × 3.\[5\]
- **Do not open per-PR detail views in the portal** if they expose golden matches.

**What to disclose publicly:**

- Config name ("recall mode"), and that it differs from the Action's default.
- Model and endpoint, sampling (T=1.0, top_p=0.95), reasoning budget.
- Chunk cap, lenses, the missed-issue pass, verifier on or off, finding cap.
- Measured cost and time per PR.
- That you never opened or tuned on golden data, and that your dev set is 40 PRs labelled from human comments.
- Whether you used any third-party, golden-derived aggregate statistics.
- How many configurations you tested. The portal shows this anyway.\[4\]

## 5. Honesty check: where grounded F1 diverges from useful review

**The headline metric rewards volume.** Grounded precision ignores unmatched findings, so extra findings cost nothing on the headline number unless they match a golden FP.\[7\] About 43% of golden findings are labelled FP (2,009 of 4,632, per the third-party tally).\[1\]

**The golden set is shaped by LLM output.** It is ~98% LLM-produced, 46% from one model, ~45% from Copilot code review producers, and majority low-severity.\[1\] Copying a thorough LLM reviewer's breadth, nits included, is what the benchmark pays for. A real team may experience that as noise.

**Independent evidence shows the same trade-off:**

- CR-Bench found its recall-raising Reflexion loop cut signal-to-noise from 5.11 to 1.95.\[29\]
- OpenCodeReview reports Claude Code reaching its highest recall (28.9%) with 4,580 comments on 200 PRs at 7.23% precision.\[30\]

**Augmented metrics (published by ReviewBench):**

| Entry | Grounded F1 | Augmented P | Augmented F1 |
|---|---|---|---|
| Laude RevEngine Prime | 59.6 | 90.8 | 75.4 |
| block/review sol-opus, gate off | 47.7 | 91.1 | 59.4 |
| Klaussy strict | 44.1 | 81.0 | 56.7 |
| Klaussy default | 35.9 | 88.1 | 46.7 |
| Copilot Balanced | 40.1 | 87.4 | 49.7 |
| DeepSeek Harness | 39.9 | 92.1 | 52.4 |
| Devin | 37.0 | 80.4 | 47.3 |
| Qodo | 35.1 | 86.8 | 44.5 |
| Codex GPT-5.6 Sol Max | 30.4 | 85.8 | 37.8 |
| Greptile | 27.2 | 85.6 | 32.8 |
| Cursor | 17.3 | 89.3 | 21.7 |
| **TraceHound v2 (25-PR test)** | **16.0** | **52.5** | **23.0** |

**What the table means:**

- **The leaders did not buy recall with noise.** RevEngine Prime and block/review have the highest augmented precision on the board as well as the highest recall.\[5\]
- **Klaussy's nits switch is the honest counterexample.** It bought +8.2 grounded F1 and +10.0 augmented F1 for −7.1 points of augmented precision.\[5\]
- **TraceHound's real problem is finding quality.** Its augmented precision is 28–40 points below every published entry before any recall push. Its grounded precision of 70.6 means ~30% of its matched findings land on golden FPs.
- **Doubling volume with the same generator could push augmented precision under 50%.** That would be visible on the public row, and would mean most comments are wrong.

**My recommendation:**

- Set a pre-registered floor of augmented precision ≥ 60 on your dev-set proxy.
- Keep the disprove-only filter available if the floor is breached.
- Publish recall mode as a benchmark configuration, not as what TraceHound ships to users by default.

## Caveats

- Every effect size in Section 3 is an extrapolation from other systems and stronger models. None has been measured on Nemotron. Your own baseline comes from one 25-PR run, so its uncertainty is several points.
- The contract, matcher prompt, classifier prompt and extraction doc were not opened directly. Their content here is quoted secondhand from a pinned commit (aee9318) and may have changed.
- Golden-set composition figures (producer shares, the 0% inferred-action count, Semgrep's 8 TPs, ~43% FP share) come from one LLM-authored third-party analysis that read `golden/`. They are plausible and internally consistent, but unaudited.
- Leaderboard figures come from a third-party mirror (BenchLM, 19 Oct 2026) because review-bench.ai could not be opened. Entries newer than that capture may exist.
- Cost estimates assume ~9k input and ~2.5k output tokens per call. Nemotron's reasoning-token use and Token Factory rate limits could move these materially. Measure on the dev set before the final.

## Sources

1. [Research: ReviewBench — what it would take to benchmark code-review there, and whether it would measure code-review at all · Issue #282 · JPHutchins/code-review · GitHub](https://github.com/JPHutchins/code-review/issues/282)
2. [ReviewBench: An open benchmark for AI code review](https://github.blog/ai-and-ml/github-copilot/reviewbench-an-open-benchmark-for-ai-code-review/)
3. [ReviewBench · GitHub](https://github.com/review-bench/)
4. [GitHub - review-bench/ReviewBench: ReviewBench is an open, reproducible benchmark for evaluating AI code review systems on real-world pull requests.](https://github.com/review-bench/ReviewBench)
5. [ReviewBench Leaderboard & Scores — October 2026](https://benchlm.ai/benchmarks/reviewbench)
6. [Compare refactoring, selective replacement, redesigned core and full rewrite · Issue #1665 · existential-birds/daydream · GitHub](https://github.com/existential-birds/daydream/issues/1665)
7. [ReviewBench/docs/METHODOLOGY.md at main · review-bench/ReviewBench](https://github.com/review-bench/ReviewBench/blob/main/docs/METHODOLOGY.md)
8. [Onboard OhMyBug (ohmybug) by review-bench-api\[bot\] · Pull Request #88 · review-bench/ReviewBench · GitHub](https://github.com/review-bench/ReviewBench/pull/88)
9. [feat(review): docs, tests and accessibility lenses; coverage checks by steph-dove · Pull Request #123 · steph-dove/klaussy-agents](https://github.com/steph-dove/klaussy-agents/pull/123)
10. [Final run pending approval: Klaussy klaussy-claude-sonnet-5-5-1 (strict config) · Issue #82 · review-bench/ReviewBench · GitHub](https://github.com/review-bench/ReviewBench/issues/82)
11. [Copilot tops GitHub's own AI code review benchmark. An independent one tells a different story. - The New Stack](https://thenewstack.io/github-reviewbench-code-review/)
12. [GitHub - lakeday-org/perch: Semantic code linting with Decision Models · GitHub](https://github.com/lakeday-org/perch)
13. [GitHub - block/review: An agentic code reviewer](https://github.com/block/review)
14. [GitHub - laude-institute/rev-engine: Agentic review for the age of agentic code.](https://github.com/laude-institute/rev-engine)
15. [GitHub - laude-institute/rev-engine-prime · GitHub](https://github.com/laude-institute/rev-engine-prime)
16. [Bug bot efficiency - Discussions - Cursor - Community Forum](https://forum.cursor.com/t/bug-bot-efficiency/149046)
17. [bench: ReviewBench feasibility spike (cc-02) by SunnyBagal · Pull Request #42 · SunnyBagal/TraceHound · GitHub](https://github.com/SunnyBagal/TraceHound/pull/42)
18. [GitHub - almaoficial/alma-reviewbench · GitHub](https://github.com/almaoficial/alma-reviewbench)
19. [rb-01: ReviewBench agent image (adapter, Dockerfile, build + smoke workflows) by SunnyBagal · Pull Request #50 · SunnyBagal/TraceHound](https://github.com/SunnyBagal/TraceHound/pull/50)
20. [feat(review): give the headless reviewer the PR's title and description by steph-dove · Pull Request #98 · steph-dove/klaussy-agents · GitHub](https://github.com/steph-dove/klaussy-agents/pull/98)
21. [Onboard DeepSeek Harness Reviewer (deepseek-harness-reviewer) by review-bench-api\[bot\] · Pull Request #65 · review-bench/ReviewBench · GitHub](https://github.com/review-bench/ReviewBench/pull/65)
22. [Final submission failed during scoring; round 3 output unavailable · Issue #73 · review-bench/ReviewBench · GitHub](https://github.com/review-bench/ReviewBench/issues/73)
23. [nvidia / nemotron-3-super-120b-a12b](https://docs.api.nvidia.com/nim/reference/nvidia-nemotron-3-super-120b-a12b)
24. [NVIDIA Nemotron 3.5 Super VL: Run Locally Guide](https://unsloth.ai/docs/models/nemotron-3.5-super-vl)
25. [Nebius Token Factory models, pricing, and API docs](https://models.opencode.ai/providers/nebius/)
26. [Nebius Token Factory](https://tokenfactory.nebius.com/)
27. [Nebius Token Factory model catalog](https://tokenfactory.nebius.com/model-catalog.md)
28. [Nebius Token Factory - Models & Pricing - LLM24](https://llm24.net/provider/nebius)
29. [How Accurate Is AI Code Review? False Positives, Recall, and Trust](https://www.augmentcode.com/guides/ai-code-review-accuracy)
30. [OpenCodeReview: Determinism over Non-Determinism for Cost-Effective Agent-Based Code Review](https://arxiv.org/pdf/2608.09290)
