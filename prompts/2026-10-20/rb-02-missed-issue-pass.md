# rb-02: missed-issue pass, ReviewBench candidate 6 (`pool-raw-missed`)

**Where:** Claude cloud (claude.ai/code), a **new session** on TraceHound. Start it only after #50 (with rb-01b) has merged.
**Spend:** $0. There's no Nebius key in this session, and no model calls are made.
**Hard cutoff:** this must be merged and smoke-tested by **23 Oct 12:00 IST**. Otherwise candidate 6 is dropped (P008 amendment 2).

Paste everything below the line.

---

You're adding one reviewer stage to TraceHound and one pipeline to its ReviewBench image.

**The idea:** after the two existing reviewers have pooled their findings, one more call per diff chunk is shown that pool and asks only for defects the pool doesn't already contain.

**Evidence:** CR-Bench (arXiv 2603.11078) found that a missed-issue pass raised recall by about 6–9 points on closed models. Nobody has measured it on Nemotron. That's what ReviewBench's test runs will decide, under pre-registered rules in the brain's `decisions/P008-reviewbench-preregistration.md`, amendment 2. Read that amendment first.

## Blindness (applies to every TraceHound job)
- Never open, read, grep or copy ReviewBench's `golden/`, its judge or classifier prompts, or any test-set PR's expected issues.
- Never open `linea-seeded`, `recall-seeded` or `tracehound-seeds`.
- Don't read Cal.com benchmark comment text.
- **Write the prompt from the idea above and the general defect categories only.** Don't tune it on any dataset, and don't make any model call.

## Step 0
- `git fetch origin && git checkout -b rb/missed-v1 origin/main`.
- Main contains rb-01b. Check: `config/prices.json` has `nvidia/Nemotron-3.5-Super-VL`, and `scripts/reviewbench/entry.ts` exists.
- `git status --porcelain` is empty.
- Record the sha256 of the three frozen prompts: `REVIEW_PROMPT_SHA256`, `REVIEW_CTX_PROMPT_SHA256` and verify-ctx's. They must be unchanged at the end.

## Allowed paths
- New files: `packages/analyzer/src/review/prompt-missed.ts`, `missed.ts` and `missed-cli.ts`, plus their tests under `packages/analyzer/test/`.
- `packages/analyzer/src/bin.ts`: one new command branch, `review-missed`, modelled on `review-ctx`.
- `scripts/reviewbench/**`, `docs/bench/reviewbench.md`, and `.github/workflows/reviewbench-smoke.yml` (add the pipeline option).

**Not allowed:**
- `packages/action/**`. The Action does not get this stage.
- Any existing prompt file or reviewer. `review.ts`, `review-ctx.ts`, `prompt.ts`, `prompt-ctx.ts` and every `verify*` file stay byte-identical.

## The stage: `tracehound review-missed`
```
tracehound review-missed --pr <input.json> --findings <pool.json> --repo <git dir> --head <sha> --out <file> [--model <id>] [--reasoning on|off]
```
- `pool.json` is `{ "comments": Finding[] }` (path, line, body), as the pipeline already holds them.
- **Reuse review-ctx-v1's machinery; don't copy it.** That means `parseDiff`, `chunkDiff`, `buildChunkContext` and the git source, `parseReviewReply`, the one retry, `CHUNK_FAILED`, `DiffIndex` line validation, and `REASONING_ON`. Temperature 0 and `max_tokens` 32,000, the same as review-ctx.
- **Per chunk, the user message holds:**
  - the chunk;
  - the same context block review-ctx-v1 builds;
  - a numbered list of the pooled findings whose path is in that chunk's files, as `path:line: body` with each body cut to 400 characters. If there are none, write "(none)".
- **The system prompt (`review-missed-v1`) says, in substance:**
  - the listed findings are already reported; don't repeat or rephrase them;
  - look for real defects the list missed, across correctness, error handling and reliability, security, API or contract breaks, concurrency, resource leaks, and missing or wrong tests;
  - every finding must point at an added or context line of this diff;
  - no style or naming comments;
  - if nothing is missed, return an empty list.
  - It uses the **same reply format** as review-diff-v1, so `parseReviewReply` validates it unchanged.
- **Output:** the same `ReviewOutput` shape as `review`, so `scripts/reviewbench/entry.ts`'s `readReview` accepts it. Export `REVIEW_MISSED_VERSION = "review-missed-v1"` and `REVIEW_MISSED_PROMPT_SHA256`.

## The pipeline: `pool-raw-missed` in `scripts/reviewbench/entry.ts`
1. Run `poolRaw(...)` exactly as candidate 2 does.
2. Write the pool to a temp file and run `review-missed` with the same model. Give it the time that's left minus `X2_MARGIN_MS`.
3. Result = the pool, then each missed finding that `agrees(...)` (decision 058's rule) with no pool finding.
4. Log `pool N, missed M, new K`.
5. **If the missed stage fails or times out, return the pool alone** and log `missed: failed (<reason>)`. It never fails a PR that pool-raw would have passed.

Then:
- Add `pool-raw-missed` to `PIPELINES` and to the smoke workflow's choices.
- Keep `MAX_FINDINGS = 40`. Pool findings come first, so if the cap cuts anything, it cuts missed findings.

## Tests (fake client, no network)
- The findings list is rendered per chunk, scoped to that chunk's files, with bodies cut at 400 characters.
- A missed finding that agrees with a pool finding is dropped; a new one is kept.
- A missed-stage failure or timeout returns exactly the pool.
- An invalid line (not on the diff) is dropped by the existing validation.
- The three existing prompt sha256 values are unchanged.
- Budget: with the real `config/prices.json`, two concurrent review-missed reservations on 3 Super and on 3.5 fit under the default `TRACEHOUND_BUDGET_RUN_USD=1`. The findings list makes the input larger.

## Docs
In `docs/bench/reviewbench.md`, add:
- candidate 6;
- the cost estimate (about 1.5× pool-raw: one extra call per chunk);
- the frozen `REVIEW_MISSED_PROMPT_SHA256`;
- one line: "the Action does not run this stage".

## Finish
- Run the analyzer and adapter tests plus typecheck. Everything must pass.
- Small commits; push `rb/missed-v1`; open the PR as a **draft**.
- **Put `REVIEW_MISSED_PROMPT_SHA256` in the first line of the PR description.** The chat records it in P008 before any test run.

Report in under 25 lines:
- test counts;
- the sha256;
- confirmation that the three frozen prompt hashes are unchanged;
- anything you couldn't do.

### After merge (for Sunny, not this session)
- Rebuild the image on main.
- Run `ReviewBench smoke` with `pool-raw-missed` on 3 Super. It must be green, show `new K` in the log, and cost a few cents.
- Then register the new digest in the portal.
