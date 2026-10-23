# 069b: intervals for the v2 gaps: final report (Sunny lane)

PR #60 (draft, not merged): https://github.com/SunnyBagal/TraceHound/pull/60. Head `aec07e3`. CI `test-and-build` passes (14m58s), and so do the Vercel checks. No model or network call was made, so the cost is $0.

## Step 0: all checks passed
- `git fetch origin` ran, and `git status --porcelain` was empty.
- main fast-forwarded to `origin/main` = `5fb8cdc`.
- The `GIT_*` env check printed nothing.
- These records exist, with per-PR score files under judge-v1 and judge-x1. Their counts are 10 PRs each on Cal.com and 40 each on dev, 0 JUDGE_FAILED:
  - Cal.com run 4;
  - Cal.com run 1, with all five tools and 3 Super's pool-verify-v1;
  - dev 7r and dev 8;
  - dev run 1, for the tools and 3 Super.
- Every per-PR sum equals its published total in `v2-calcom.md` and audit §3.1, under both judges.
- I read `v2-calcom.md`, `audit-cc01.md` §3 and its appendix, `results.md`, and decision 066.

## Commits
- `974febd` docs: save the 069b brief (v2 intervals)
- `5e16e2b` docs: pre-register the v2 intervals (069b)
- `36ce0f3` bench: PR-level paired bootstrap and the audit's interval for the v2 gaps (069b)
- `59c766b` docs: v2 intervals result (069b)
- `aec07e3` docs: 069b note under decision 069

## Plan before the first bootstrap
The plan commit `5e16e2b` was pushed at 16:07:42Z on 23 Oct. The first bootstrap ran at 16:09:13Z, 91 s later.

## Tests
- Local: analyzer 913 passed / 6 skipped, action 237, viewer 188. Typecheck is clean.
- `intervals.test.ts` has 10 tests on synthetic counts, covering:
  - a known-gap fixture;
  - seed determinism;
  - the JUDGE_FAILED drop;
  - pairing by key;
  - the wording rule.

## Gap table
Each gap is F(TraceHound v2) − F(other), in points. The primary interval is a PR-level paired bootstrap: 10,000 resamples, seed 69, 0 JUDGE_FAILED dropped.

**Secondary method (audit §3.2 (b)): not reproduced, so skipped for every gap, as pre-registered.**
- The v1 comment-level bounds matched exactly.
- Six inflated bounds were 0.1–0.2 off.
- Under x1, Greptile's lower bound was 0.4 off and Copilot's upper bound 0.2 off.
- The bar was 0.1.

| set | judge | v2 − | observed | primary 95% | share > 0 | secondary |
|---|---|---|---|---|---|---|
| Cal.com | v1 | Augment | −16.0 | [−30.9, +3.5] | 0.044 | not reproduced |
| Cal.com | v1 | Cursor | −9.5 | [−30.2, +16.7] | 0.220 | not reproduced |
| Cal.com | v1 | Greptile | +7.1 | [−15.8, +36.8] | 0.725 | not reproduced |
| Cal.com | v1 | Copilot | +11.0 | [−6.2, +27.7] | 0.905 | not reproduced |
| Cal.com | v1 | CodeRabbit | +20.4 | [+11.8, +34.3] | 1.000 | not reproduced |
| Cal.com | v1 | 3 Super (run 1) | −1.2 | [−16.0, +12.2] | 0.414 | not reproduced |
| Cal.com | x1 | Augment | −14.6 | [−28.5, +4.4] | 0.054 | not reproduced |
| Cal.com | x1 | Cursor | −8.1 | [−27.9, +17.5] | 0.252 | not reproduced |
| Cal.com | x1 | Greptile | +3.5 | [−17.1, +31.2] | 0.621 | not reproduced |
| Cal.com | x1 | Copilot | +7.7 | [−8.3, +20.5] | 0.850 | not reproduced |
| Cal.com | x1 | CodeRabbit | +21.0 | [+1.4, +37.0] | 0.981 | not reproduced |
| Cal.com | x1 | 3 Super (run 1) | +0.2 | [−13.4, +13.1] | 0.506 | not reproduced |
| all 50 | v1 | Augment | −14.0 | [−22.6, −4.7] | 0.001 | not reproduced |
| all 50 | v1 | Cursor | −1.6 | [−12.6, +10.7] | 0.400 | not reproduced |
| all 50 | v1 | Greptile | +2.8 | [−7.1, +13.2] | 0.709 | not reproduced |
| all 50 | v1 | Copilot | +17.0 | [+6.0, +28.0] | 0.999 | not reproduced |
| all 50 | v1 | CodeRabbit | +12.2 | [+3.8, +20.6] | 0.996 | not reproduced |
| all 50 | v1 | 3 Super (row (i)) | +11.0 | [+1.8, +20.1] | 0.991 | not reproduced |
| all 50 | x1 | Augment | −10.5 | [−19.8, −0.3] | 0.022 | not reproduced |
| all 50 | x1 | Cursor | −3.5 | [−13.7, +8.3] | 0.275 | not reproduced |
| all 50 | x1 | Greptile | +0.3 | [−9.5, +10.7] | 0.520 | not reproduced |
| all 50 | x1 | Copilot | +13.4 | [+1.2, +25.6] | 0.984 | not reproduced |
| all 50 | x1 | CodeRabbit | +10.4 | [+1.1, +19.2] | 0.985 | not reproduced |
| all 50 | x1 | 3 Super (row (i)) | +7.2 | [−3.0, +17.4] | 0.918 | not reproduced |

All-50 ranks are point estimates. Under both judges v2 is 3rd of 6: v1 F 52.2%, x1 F 55.4%.

Report-only sensitivity, with dev 8 in place of 7r:
- Under v1, every sentence stays the same.
- Under x1, the Augment interval becomes [−19.2, +1.3], so that sentence becomes "statistically tied".

## Allowed sentences
- **Cal.com (10 blind PRs), both judges:** TraceHound v2 is statistically tied with Augment, Cursor, Greptile and Copilot, and ahead of CodeRabbit. It is statistically tied with 3 Super's run 1. Its rank, 3rd of 6, is a point estimate.
- **All 50 PRs, both judges:** TraceHound v2 is behind Augment, statistically tied with Cursor and Greptile, and ahead of Copilot and CodeRabbit. Its rank, 3rd of 6, is a point estimate.
- **Against 3 Super on all 50:** ahead under bench-judge-v1 (scorer of record), statistically tied under bench-judge-x1. A sentence has to name the judge.

## Caveats and what I couldn't do
- The secondary method was not reproduced, so there is no inflated interval for any v2 gap.
- A 10-PR bootstrap probably under-covers. Cal.com's "ahead of CodeRabbit" rests on it (x1 lower bound +1.4).
- The intervals cover PR sampling only, not run-to-run or judge spread.
- 24 gaps, with no multiplicity correction.
- The dev PRs in the all-50 set are the ones v2 was selected on.
- `scripts/bench/intervals.ts` makes `freezeCheck069` fail from now on. That is acceptable because no further Cal.com run is allowed.
- `results.md`, the README and `viewer/` are unchanged.
