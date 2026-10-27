# 071b report: Ultra dev run 11 folded into PR #63, plus Devin's fixes

PR #63 is open and ready for review, with head `b5a5f2e`. CI is green (test-and-build, review, Devin Review, Vercel). Nebius spend this session: $0, with no model calls.

## Step 0
- `git fetch` ran, and the tree was clean. Local HEAD was `35df43a`. `origin/build/071-dev-ultra` was `6bd72e1`. The branch was 1 commit ahead and 0 behind, and `git diff --stat 6bd72e1 35df43a` showed only `docs/prompts/071-followup-run11.md`.
- I copied the stale prompt to `runs/071-followup-run11.stale.md` and removed it in `188cbe4` ("docs: drop the stale 071 follow-up prompt (session stopped before running it)").
- The GIT_* env check printed nothing, both here and before every gate.

## Run 11
- `runs/071-chain.log`: `run 11 exit 0` and `chain done` at 2026-10-24T14:08:06Z. Status is complete.
- All four baseline checks say ok:
  - pool-verify-v1 3 Super: 42.28 against 42.3
  - pool-verify-v1 3.5 Super: 50.36 against 50.4
  - pool-raw 3 Super: 38.16 against 38.2
  - pool-raw 3.5 Super: 53.87 against 53.9

## Tables (P / R / F, n = 74)

| row | judge | 3 Super mean (5) | 3.5 Super mean (2) | Ultra run 10 | Ultra run 11 | Ultra mean (2) |
|---|---|---|---|---|---|---|
| pool-verify-v1 | v1 | 40.5 / 44.3 / 42.3 | 54.8 / 46.6 / 50.4 | 34.8 / 54.1 / 42.3 | 43.2 / 64.9 / 51.9 | 39.0 / 59.5 / 47.1 |
| pool-verify-v1 | x1 | 50.9 / 51.9 / 51.3 | 60.5 / 50.7 / 55.1 | 45.9 / 67.6 / 54.6 | 47.1 / 66.2 / 55.1 | 46.5 / 66.9 / 54.9 |
| pool-raw | v1 | 30.0 / 52.7 / 38.2 | 48.0 / 61.5 / 53.9 | 31.2 / 64.9 / 42.1 | 34.3 / 66.2 / 45.2 | 32.7 / 65.5 / 43.6 |
| pool-raw | x1 | 37.3 / 63.8 / 47.0 | 49.2 / 62.2 / 54.9 | 39.0 / 77.0 / 51.8 | 40.0 / 70.3 / 51.0 | 39.5 / 73.6 / 51.4 |

## Intervals (paired bootstrap, 10,000 resamples, seed 71, both Ultra runs)
- pool-verify-v1 v1: Ultra − 3.5 Super −3.3 [−12.8, +6.8]; Ultra − 3 Super +4.8 [−2.0, +12.0]
- pool-verify-v1 x1: Ultra − 3.5 Super −0.3 [−9.6, +9.5]; Ultra − 3 Super +3.6 [−3.1, +10.2]
- pool-raw v1: **Ultra − 3.5 Super −10.2 [−17.7, −3.2]: this interval excludes 0, so Ultra is worse here.** Ultra − 3 Super +5.5 [−0.8, +12.1]
- pool-raw x1: Ultra − 3.5 Super −3.5 [−12.8, +5.6]; Ultra − 3 Super +4.4 [−2.1, +10.7]
- No other interval excludes 0. Nothing is adopted.

## Run 11 operations
- 09:52:04Z to 14:08:05Z (4 h 16 min). Wall time per PR: median 252 s, max 1,475 s.
- 361 NVIDIA API requests, of which 57 were HTTP 429 (run 10 had 0 of 306).
- 304 Ultra calls ledgered; 5 failed without reported usage (timeout, network or 5xx).
- 4 PRs were rerun. 1 still had a failed request after its rerun: 1 failed chunk, `HTTP 500: Internal error while making inference request`.
- 0 VERIFY_FAILED. At max_tokens: 0 reviewer calls and 0 verifier calls.
- Tokens per PR for Ultra over both runs (n = 80): input median 48,316; output median 14,151, mean 17,465.
- Run 10's PR rerun count was 2 in the old doc; `runs/071-dev10.log` says 3, so I corrected it.
- 3.5 Super run 7r's calls at max_tokens were 32 in the old doc; the report says 31, so I corrected it.

## Spend and tokens
- Judges (Nebius): run 11 $0.0923. 071 total $0.1934 (run 10 $0.1011).
- Ultra tokens, all at $0:

| run | input | output |
|---|---|---|
| 10 | 2,491,296 | 688,589 |
| 11 | 2,537,506 | 708,583 |
| total | 5,028,802 | 1,397,172 |

## Devin's findings
1. **Baseline mismatch didn't stop the script.** Fixed in `7c4aca7`: the script now prints only the check lines and exits 1.
2. **Token Factory Ultra calls bypassed the caps.** Fixed in `2f3a7eb`: the $0 price holds only on the NVIDIA route. Unflagged, the id prices at `placeholderFallback`, and the `prices.json` `_readme` says so.
3. **Verifier truncations weren't counted.** Reproduced first: every run ledgers its verifier calls as `review`. Fixed in `7c4aca7`:
   - Verifier calls are now identified by matching token counts to the verdict usage in the attempt records. The ledger format is unchanged.
   - In every dev run, the identified verifier calls equal the verdicts' calls.
   - The report has a new "verifier at max_tokens" column: 1 in 3.5 Super run 8, 0 in both Ultra runs.
4. **Cache entries crossed provider routes.** Fixed in `2f3a7eb`: NVIDIA-routed requests use an `nvidia-api:` cache namespace, and Token Factory keys are unchanged.

Devin marked all 4 resolved. Its re-review then raised one new finding against fix 3: equal token totals could be over-counted as verifier truncations. I fixed it in `b5a5f2e` by matching with multiplicity, added tests, and replied on that thread. The report is byte-identical before and after.

## github-actions comments, left open
Both are on `scripts/bench/ultra-smoke.ts` and are out of scope:
- Line 26: the script requires `NEBIUS_API_KEY` for an NVIDIA-only smoke run.
- Line 51: the catch handler assumes an `Error`.

## Report regeneration
- `runs/071-report-final-2.txt` equals `runs/071-report-final.txt` except for the new verifier column in the operations table.
- `packages/analyzer/test/dev-ultra.test.ts` checks this against the saved report. That test runs only where the records exist, so CI skips it.

## Commits after `6bd72e1`
- `35df43a` docs: 071 follow-up prompt (stale, from the stopped session)
- `188cbe4` docs: drop the stale 071 follow-up prompt
- `242c37d` docs: 071b prompt
- `2f3a7eb` llm: Ultra's $0 price and cache namespace hold only on the NVIDIA route
- `7c4aca7` bench: dev-ultra stops on a baseline mismatch and counts verifier truncations
- `cdd7c08` docs: 071 result on Ultra dev runs 10 and 11
- `e3a8cef` docs: decision 071 and its feedback entry cover run 11
- `87cad0e` docs: 071 feedback entry states run 11's judge spend on its own
- `b5a5f2e` bench: dev-ultra matches verifier calls with multiplicity

## Diff stat against `6bd72e1`
9 files, +590 / −81:
- FEEDBACK.md
- config/prices.json
- docs/bench/dev-ultra.md
- docs/decisions.md
- docs/prompts/071b-run11-and-devin-fixes.md
- packages/analyzer/src/llm/client.ts
- packages/analyzer/test/dev-ultra.test.ts
- packages/analyzer/test/llm-nvidia-api.test.ts
- scripts/bench/dev-ultra.ts

## Gates
- **First `pnpm test`:** 5 contracts tests failed at about 6 s each, past the 5 s default timeout:
  - `contracts-keyed`
  - `contracts-fixtures` (2 tests)
  - `contracts-typed`
  - `contracts-findings`

  I reran those 4 files with `--testTimeout 30000` and all 130 tests passed.
- **Final `pnpm test`, after `b5a5f2e`:** clean.
  - action: 237 passed
  - analyzer: 996 passed, 2 skipped
  - viewer: 188 passed
- **`pnpm typecheck`:** clean, both times.

## PR state
- Head `b5a5f2e`. The PR was already ready for review, so I skipped `gh pr ready`.
- CI is green. Not merged.

## Not done
- The two github-actions threads are left open, as the brief says.
- The 071b commits after `87cad0e` and the new Devin thread came after the brief's step list. They're described above.
