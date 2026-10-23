# 069 report: the v2 blind Cal.com run (second look)

Draft PR **#59** · head `7f757c1fd6a2eb27e0dcf03cc1fa0eae5364ac5a` · CI test-and-build pass (14m52s). Not merged.

Commits:
- `d646a0f` docs: save the 069 brief (v2 blind Cal.com run)
- `713f515` bench: Cal.com run 4 (v2) in the test-set runner, gate 069 and freeze check
- `b2ec214` docs: pre-register the v2 blind Cal.com run (decision 069)
- `7f757c1` docs: v2 blind Cal.com run result (decision 069)

Tests (local, before the runner commit): action 237 passed; analyzer 903 passed, 6 skipped; viewer 188 passed. Typecheck clean.

## Step 0: all passed
- I fast-forwarded main to origin/main `8712273`. The working tree was clean.
- `review-freeze-2^{}` = `0aeef06fa8742cc8a36581c3894e554b8748c965`.
- `git diff review-freeze-2 HEAD --stat` listed only `.github/workflows/tracehound-review.yml`.
- `## 069` count 0, `## 068` count 1.
- Cal.com inputs (counts only): 10 PR files, the clone, a heads file with 10 entries, and the golden file exist.
- Runs 1–3 are complete, and no `run-4*` existed.
- The ledger was $33.81 (≤ $34.50). The `GIT_*` env check printed nothing.

## Pre-registration
`b2ec214` was pushed at 13:38:03Z on 23 Oct. The first 3.5 call on Cal.com came at 13:38:51Z, 48 s later. All three prompt sha256s were recomputed and match: `f77c2031…4849`, `0c4dcdcf…89e0`, `75703fc6…3e70`.

## Routing check
One review-diff-v1 request on one small dev PR, with cache reads off. It went to `api.tokenfactory.us-north1.nebius.com`, returned HTTP 200, took 20.6 s and cost $0.0057.

## Run
- Complete: 10/10 PRs, 0 reruns, 0 restarts, 0 JUDGE_FAILED.
- 13:38:51Z–14:02:27Z, $0.7228 of the $3 allowance.
- `run.json` records the decision, the gate 069, `review-freeze-2` and the model.

## Headline: Cal.com, n = 23 (3.5 rows = run 4; other rows = run 1 unchanged)

| row | v1 P | v1 R | v1 F | x1 P | x1 R | x1 F |
|---|---|---|---|---|---|---|
| **pool-verify-v1 (3.5)** | 63.2% (12/19) | 52.2% (12/23) | **57.1%** | 66.7% (12/18) | 52.2% | **58.5%** |
| review-diff-v1 (3.5) | 66.7% (12/18) | 52.2% | 58.5% | 70.6% (12/17) | 52.2% | 60.0% |
| pool-raw (3.5) | 59.1% (13/22) | 56.5% | 57.8% | 65.0% (13/20) | 56.5% | 60.5% |
| pool-verify-v1 (3 Super, run 1) | 56.0% (14/25) | 60.9% | 58.3% | 56.0% (14/25) | 60.9% | 58.3% |
| augment | 83.3% | 65.2% | 73.2% | 83.3% | 65.2% | 73.2% |
| cursor | 81.3% | 56.5% | 66.7% | 81.3% | 56.5% | 66.7% |
| greptile | 58.8% | 43.5% | 50.0% | 64.7% | 47.8% | 55.0% |
| copilot | 35.7% | 65.2% | 46.2% | 40.0% | 69.6% | 50.8% |
| coderabbit | 34.6% | 39.1% | 36.7% | 36.0% | 39.1% | 37.5% |

3 Super pool-verify-v1, Cal.com runs 1–3 (min / median / max):
- v1: F 51.3 / 52.8 / 58.3%, P 46.7 / 55.6 / 56.0%.
- x1: F 47.3 / 57.1 / 58.3%.

v2 is 3rd of 6 under both judges (a point estimate). 11 of its 30 comments were judged duplicates (3 Super run 1: 4).

## Predictions: all right
- (a) F (v1) 57.1% ≥ 52.8%: **right**.
- (b) P (v1) 63.2% ≥ 55.6%: **right**.
- (c) 1 of 10 PRs had a failed reviewer chunk, ≤ 3: **right**.

## Failed chunks

| PR (order) | stage | chunk | failure |
|---|---|---|---|
| PR 4 | review-diff-v1 | 0 of 1 | the reply is not valid JSON; after one retry: the reply is not valid JSON |
| PR 4 | review-ctx-v1 | 0 of 1 | the reply is not valid JSON; after one retry: the reply is not valid JSON |

- 2 of 20 chunks failed, and PR 4 was scored with no findings. 0 request failures, 0 VERIFY_FAILED.
- 10 of 64 3.5 responses hit exactly 32,000 completion tokens.
- Kept findings per PR: 4, 2, 1, 0, 9, 2, 6, 2, 3, 1. More than 10: **0**. More than 15: 0.
- Cost per PR: median $0.0717, max $0.1298 (3 Super run 1 median: $0.0391). Output tokens per PR: median 55,933, max 128,000.

## All 50 (v2), 066's method (TP and FP summed, FN = 97 − TP)

| reviewer | v1 rank | v1 F | x1 rank | x1 F |
|---|---|---|---|---|
| Augment | 1 | 66.3% | 1 | 65.9% |
| Cursor | 2 | 53.8% | 2 | 58.8% |
| **TraceHound v2: dev 7r + Cal.com run 4** | 3 | **52.2%** (P 56.6% 47/83, R 48.5%) | 3 | **55.4%** (P 61.3% 49/80, R 50.5%) |
| TraceHound v2: dev 8 + Cal.com run 4 | 3 | 51.7% | 3 | 56.5% |
| Greptile | 4 | 49.5% | 4 | 55.1% |
| CodeRabbit | 5 | 40.0% | 5 | 45.0% |
| Copilot | 6 | 35.2% | 6 | 42.0% |

066's 3 Super row (i) was 4th: 41.2% under v1 and 48.1% under x1.

## Spend
- Job: **$0.7285** (routing check $0.0057, run $0.7228). The ledger went from $33.8056 to $34.5341 of the $45 cap.

## Deviations and notes
- The brief says the Action posts at most 10 findings. Its default `max-comments` is actually **15** (`action.yml`); only the dogfood workflow sets 10. The plan and result report both thresholds. Neither matters on this set, because no PR had more than 10.
- "Name the script" for the all-50 table: 066 had no script; its tables are hand sums of published counts. This job did the same sums with a throwaway script under `runs/`, which is not committed: a new file under `scripts/bench` would break run 4's freeze check. On run 1's counts the script reproduces 066's row (i) exactly.
- The plan lists two extra differences from the Action: the Action marks partial reviews, and its fixes run on 3 Super.
- `results.md`, the README and the viewer are not touched.
