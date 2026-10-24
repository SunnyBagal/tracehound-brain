# 070 report (Sunny lane, relay): recall-v3

- **PR #62** (draft): https://github.com/SunnyBagal/TraceHound/pull/62, head `cf55841`, CI green (test-and-build 14m4s, Vercel pass).
- **Step 0:** an earlier session of this job ran it and committed the brief (`465a50c`) and the plan (`78d41c1`, P009 sections verbatim) on top of main `f488cef` (contains #60 and #61). This session found a clean tree apart from the untracked recall/ draft, the `GIT_*` check printed nothing, and the gates were green: `pnpm typecheck` passed; `pnpm test` gave analyzer 933 passed / 6 skipped, action 237, viewer 188, all passing with no timeouts; ReviewBench adapter tests 87 passed; `tsc -p scripts/reviewbench` passed.
- **Commits:** `465a50c` brief · `78d41c1` plan (P009 verbatim) · `6a7e121` recall-v3 build (CLI, lenses, reduce, adapter, smoke line, tests) · `2f9d3cf` plan: lens shas and knobs · `8ffc2a6` probe result, dev runner, results script · `cf55841` dev result, decision 070, FEEDBACK.
- **Plan vs first model call:** builder details with the lens shas were pushed in `2f9d3cf` at 2026-10-23T21:09:05Z. The first v3 model call (probe) was at 21:09:28Z. The probe result was pushed in `8ffc2a6` at 21:13:39Z, and arm S started at 21:13:42Z.
- **Lens prompt sha256:** A `0a1a29c96c2d7c6520bf47d2d4effc5fb2d359087204a4b794d9202c70f69351`, B `804e955260a19d7b75d34921a9bb065369183eca34074314db3c10b0f88d12dd`. The missed pass is review-missed-v1 unchanged (`a5a2115f…`).
- **Reasoning budget:** neither model honours one. 3 Super: `reasoning_budget: 256` was ignored (16,000 reasoning tokens, finish length); `reasoning_effort: "low"` gave 142 tokens, but that is an effort level, not a ~3,000-token budget. 3.5 Super VL: both were ignored (12.9k and 12.2k reasoning tokens). Both arms therefore used max_tokens 16,000 plus one retry with reasoning off on finish_reason=length.

**Dev table** (40 dev PRs, n = 74, 0 JUDGE_FAILED):

| arm | judge | comments | P | R | F | R without missed | R without lens B |
|---|---|---|---|---|---|---|---|
| S (3 Super) | v1 | 313 | 14.0 | 50.0 | 21.8 | 39.2 | 32.4 |
| S | x1 | 313 | 16.3 | 56.8 | 25.3 | 43.2 | 41.9 |
| V (3.5 Super VL) | v1 | 271 | 20.9 | 63.5 | 31.4 | 54.1 | 47.3 |
| V | x1 | 271 | 26.2 | 71.6 | 38.4 | 60.8 | 52.7 |

- **Findings per PR:** median 6 for both arms; max 60 (S) and 18 (V); 1 PR with 0 findings in each arm.
- **Cost per PR:** S median $0.0499, mean $0.0552, max $0.1081. V median $0.0931, mean $0.0833, max $0.1178.
- **Seconds per PR:** S median 132, max 361. V median 134, max 206.
- **Truncated lens replies:** S 27/124, V 38/124. Lens calls failed (invalid JSON after the retry, or the per-PR cap): S 30, V 45. The 3.5 missed pass failed on 22 PRs.
- **Baselines** (pool-raw dev, v1, recomputed): 3 Super runs 1/2r/4/5/6, mean P 30.0 / R 52.7. 3.5 runs 7r/8, mean P 48.0 / R 61.5.
- **Floor and pick:** S 14.0 < 20.0 and V 20.9 < 38.0, so **neither qualifies; v3 stops here** and the P008 winner gets the final. No v3 image, smoke or portal run.
- **Disclosed reading of the stop rule:** arm S's status file listed 15 "request errors". All 15 were recall-v3's own pre-registered $0.12 per-PR cap. There were no HTTP 4xx/5xx and no global budget refusal (0 ledger failures in the whole job). I read "budget refusal" as the client's global/run cap and continued to arm V. The outcome would be the same either way.
- **Measured final-run cost** (cost per PR × 657): S $36.29 mean / $32.78 median; V $54.70 / $61.15.
- **Spend:** this job used 562 Nebius requests, 0 failed, est. $5.71 (relay cap $14). The ledger is now $40.24; run commands used `TRACEHOUND_BUDGET_TOTAL_USD=60`.
- **Couldn't / didn't:**
  - The existing test expectation for the unknown-pipeline error message was left untouched. Instead, the message still lists the four old pipelines and appends "(recall-v3 is also accepted)".
  - `scripts/reviewbench/imports.test.ts` doesn't cover the new `review-recall` stage; it's an existing test, so I didn't edit it.
  - `scripts/bench/` isn't typechecked in CI.
