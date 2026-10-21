# 067b report: done. Verdict: adopt Nemotron 3.5 Super (2-run fallback)

## Step 0: all passed
- `git fetch origin` ok; `git status --porcelain` printed nothing. On `build/067-dev-3.5`, HEAD `cf5b70b`.
- `da13585` is an ancestor of origin/main (origin/main = `da13585`). The `GIT_*` env check printed nothing, here and before every gate.
- `run-7-nvidia-nemotron-3-5-super-vl/run.json`: status `running`, costUSD 0, no `scores/`.
- Ledger $25.82199 (≤ $25.83).

## Step 1: merge
- Merge commit `d73af1f`, pushed. FEEDBACK.md was the only conflicted file. I removed the three conflict markers and turned the `=======` line into one blank line. Result: the branch's "2026-10-20 · job 067" entry first, then main's "2026-10-21 · Token Factory regions" entry, word for word. The marker grep printed nothing.
- Tests green (analyzer 891 passed / 6 skipped, action 226, viewer 188) and typecheck green.

## Step 2: prompt saved
- `9d69e71`: appended verbatim under `## Resume 2 (067b)`.

## Step 3: freeze check
- The diff listed exactly `config/prices.json`, `packages/analyzer/src/bin.ts`, `packages/analyzer/src/config.ts`, `packages/analyzer/src/llm/client.ts`, `scripts/bench/run-test-set.ts` and `scripts/bench/test-set.ts`, so nothing extra.
- Commit `9b265ca`: `client.ts` added to `ALLOWED_MODIFIED_067` with an updated comment. The pinned list was updated. A new test asserts that `baseUrlFor` keeps REVIEW_MODEL and both JUDGES models on the us-central1 URL and sends `nvidia/Nemotron-3.5-Super-VL` to us-north1. `freezeCheck067()` → ok.
- Tests 892 passed; typecheck green.

## Step 4: amendment
- Commit `164ead6`, pushed 2026-10-21T16:41:45Z. The failed-reviewer rule is cited at `packages/analyzer/src/review/review.ts:4-5` and `packages/analyzer/src/bench/score.ts:54-58`, `:121-122`.
- The first 3.5 call (the probe) was sent 2026-10-21T16:42:15.7Z, after the push.

## Step 5: probe
- One reviewPull through the runner's `buildClients` (TokenFactoryClient) on dev grafana/7, one chunk. It went to `https://api.tokenfactory.us-north1.nebius.com/v1/chat/completions` and got HTTP 200 in 16.2 s, $0.0057, 1 finding.

## Step 6: runs
| run | status | spend | allowance | wall | reruns |
|---|---|---|---|---|---|
| 7r (restart of void 7) | complete | $3.1463 | $7.9943 | 123.6 min | 1 (grafana/3, attempt 1 "retry request failed: fetch failed"; attempt 2 clean) |
| 8 | complete | $2.9410 | $4.8480 | 108.9 min | 0 |
| 9 | stopped-at-cap at PR 24/40 | $1.8906 | $1.9070 | 69.6 min | 0 |

- Run 9's refusal: `refusing review on nvidia/Nemotron-3.5-Super-VL: estimated $0.0347 would bring this run to $1.9253, over TRACEHOUND_BUDGET_RUN_USD=1.907013899999999`.
- Run 7r's allowance is $8 − $0.0057, because the void-run report counts ledger spend since void run 7 started, which includes the probe.
- Stop rule before run 8: 3 of 40 PRs had 0 findings, no request-error strings, and 7 of 40 PRs had a failed chunk. Result: continue.
- The plan's 2-run fallback applies: runs 7r and 8 decide.

## Per-run and mean (pool-verify-v1, n = 74, 0 JUDGE_FAILED)
| judge | model | run | P | R | F |
|---|---|---|---|---|---|
| v1 | 3 Super | 1 / 2r / 4 / 5 / 6 | 36.1 / 47.4 / 41.2 / 39.8 / 38.2 | 35.1 / 48.6 / 47.3 / 44.6 / 45.9 | 35.6 / 48.0 / 44.0 / 42.0 / 41.7 |
| v1 | 3 Super | mean (5) | 40.5 | 44.3 | 42.3 |
| v1 | 3.5 Super | 7r / 8 | 54.7 / 54.8 | 47.3 / 45.9 | 50.7 / 50.0 |
| v1 | 3.5 Super | mean (2) | 54.8 | 46.6 | 50.4 |
| x1 | 3 Super | mean (5) | 50.9 | 51.9 | 51.3 |
| x1 | 3.5 Super | 7r / 8 | 59.7 / 61.3 | 50.0 / 51.4 | 54.4 / 55.9 |
| x1 | 3.5 Super | mean (2) | 60.5 | 50.7 | 55.1 |

Rule: F (v1) 50.4 ≥ 45.28 pass · P (v1) 54.8 ≥ 40.52 pass · F (x1) 55.1 ≥ 51.29 pass.

## Failed-chunk PRs (attempt used)
Every string is "the reply is not valid JSON; after one retry: the reply is not valid JSON". The records hold no per-chunk completion tokens. The ledger shows 79 of 624 3.5 calls at exactly 32,000 output tokens (`max_tokens`).
| run | PRs (chunks) | VERIFY_FAILED |
|---|---|---|
| 7r | grafana/0 (diff#0, ctx#0), grafana/1 (diff#0), grafana/2 (diff#0), grafana/6 (ctx#0), keycloak/6 (ctx#0), sentry/2 (ctx#0), sentry/5 (ctx#0): 7 of 40 | 1 (discourse/7, same string) |
| 8 | discourse/8 (ctx#0), grafana/1 (diff#0, ctx#0), grafana/6 (ctx#0), keycloak/6 (ctx#0), sentry/4 (ctx#0), sentry/5 (ctx#0), sentry/9 (diff#0): 7 of 40 | 0 |
| 9 (partial) | grafana/1 (ctx#0), keycloak/0 (ctx#0), sentry/4 (ctx#2), sentry/5 (ctx#0): 4 of 23 | 2 (keycloak/1, sentry/4, same string) |

For comparison, the 3 Super runs 2r/4/5/6 had 1/0/0/0 PRs with a failed chunk.

## Bootstrap (10,000 resamples, seed 67, 40 PRs)
- v1: +8.1 points [+1.4, +14.4], 0.9% of resamples below 0.
- x1: +3.9 points [−4.2, +11.1], 16.8% below 0.

## Cost
- Pipeline per PR: 3.5 median $0.0626 / mean $0.0743 (n = 80), against 3 Super's $0.0307 / $0.0430 (n = 200).
- Output tokens median: 50,064 against 19,688.

## Spend
- Ledger $25.82199 → $33.80558, so **$7.98359** (under $8, probe included). 3.5: 624 calls, $7.870. Judge v1: 130 calls, $0.094. Judge x1: 130 calls, $0.020.

## Verdict
**Adopt 3.5 Super as the Action's default, on 2 completed runs (the plan's fallback).** All three conditions pass. The gain is in precision. The x1 interval includes 0. 3.5 costs about 2× per PR, and 7 of 40 PRs per run lose a chunk to invalid JSON.

## PR
- #56 (draft): https://github.com/SunnyBagal/TraceHound/pull/56. Head `46a678d`. CI test-and-build passed; Vercel passed.
- Commits: `d73af1f` merge main · `9d69e71` save prompt · `9b265ca` freeze-check allow client.ts + test · `164ead6` plan amendment 1 · `59dc231` dev-3-5 cost-table fix for a capped run (+ test) · `46a678d` dev-3.5.md, decision 067, FEEDBACK.
- Final gate: analyzer 893 passed / 6 skipped, action 226, viewer 188; typecheck green.

## Not done / notes
- Run 9 is incomplete (cap), so the verdict rests on 2 runs.
- Fix outside the plan: `scripts/bench/dev-3-5.ts` crashed in its cost table on run 9's partial folder (no `attempts/discourse`). The fix skips repos that are absent. Scores and the verdict were unaffected; they had printed before the crash.
- Probe and checker scripts live in `runs/` (gitignored): `runs/067-probe.ts`, `runs/067-check.ts`, `runs/067-score.txt`.
- The earlier attempt's `inbox/2026-10-21-sunny-067b.md` was left untouched.
- Not merged.
