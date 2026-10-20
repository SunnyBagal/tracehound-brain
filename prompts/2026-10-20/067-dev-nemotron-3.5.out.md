# 067 report: STOPPED at run 7, because the model id returns 404

**Status: stopped as the brief requires.** Every chat request to `nvidia/Nemotron-3.5-Super-VL` got `HTTP 404: The model `nvidia/Nemotron-3.5-Super-VL` does not exist.` That is the "wrong model id" stop at the run-7 check. Runs 8 and 9 were not started. There is no score, no table, no bootstrap interval and no verdict. Nothing was spent.

## Step 0 (all passed)
- `git fetch`; the tree was clean. main was fast-forwarded to origin/main `95f79f0` (36 commits), and HEAD = origin/main.
- `config/prices.json` has `nvidia/Nemotron-3.5-Super-VL` at 0.30/0.90.
- `^## 066` count 0, `^## 067` count 0.
- The baseline records exist: run 1 (059's records, under `reviews/` and `scores/`; it is not in `dev-repeat/`), plus `dev-repeat/run-2r`, `run-4`, `run-5` and `run-6`.
- The `GIT_*` env check printed nothing.

## Commits on `build/067-dev-3.5`
- `c913178` docs: save brief 067
- `fd306b6` bench: `--model` for dev runs 7–9 in run-test-set. Only the reviewer and verifier stages use it, and judges are unchanged. Records go to `run-<n>-<slug>/`, and run.json records the model. Runs 7–9 have their own $8 cap. `freezeCheck067` compares against `review-freeze-1` and allows a fixed list of modified files. A test pins the Cal.com path to the tag's text and to the default model.
- `c983abc` bench: `dev-3-5.ts`, the scoring script (per-run P/R/F, paired bootstrap with seed 67, the rule). No model calls.
- `7456936` bench: freeze plan `docs/bench/dev-3.5-plan.md`
- `cf5b70b` docs: Nebius feedback for 067

Tests: analyzer 881 passed / 6 skipped, action 179, viewer 186. Typecheck clean.

## Freeze timing
- The plan commit `745693679c33a2a2c586bd742e3402ca97667933` was committed and pushed at 14:49:09Z on 2026-10-20.
- The first model call was at 14:49:18Z, 9 s after the push.

## Baseline (recomputed, matches within 0.1)
- v1: F 42.28, P 40.52. x1: F 51.29.
- Per run, v1 F: 35.6 / 48.0 / 44.0 / 42.0 / 41.7 (runs 1 / 2r / 4 / 5 / 6).

## Run 7
- Launched at 14:49:18Z. The first 4 PRs all failed with CHUNK_FAILED, each rerun once. I killed the process at about 14:50:20Z.
- 35 requests were sent, with 0 successes and $0.
- `run-7-nvidia-nemotron-3-5-super-vl/run.json` was left as is: status `running`, 4 pulls, nothing scored. By 061a it is void, and it may be restarted once.
- First failure reason: `request failed: HTTP 404: The model `nvidia/Nemotron-3.5-Super-VL` does not exist.`
- `GET /v1/models` on the same key and base URL **does list** `nvidia/Nemotron-3.5-Super-VL`, next to `nvidia/nemotron-3-super-120b-a12b`. So the listed id is not callable for chat completions on this account. Possible causes: not enabled for the account, a different endpoint for a VL model, or not yet deployed. I did not probe further.

## Spend
- $0.00 for 067. The ledger stays $25.82 of $45.

## Table, bootstrap, verdict
- None. There is no candidate data. 3 Super stays the default until 3.5 can be called.

## PR
- None opened. Step 6 was not reached.
- Branch `build/067-dev-3.5` is pushed at head `cf5b70b937d04f9a6016a84cfa76a4c84a3219f3`.
- Decision 067 was not appended, and `docs/bench/dev-3.5.md` was not written.

## To resume
1. Find the callable id or endpoint for 3.5 Super (Token Factory console or support).
2. If the id differs, `config/prices.json` needs that key. That file is outside this lane's allowed paths.
3. Restart run 7 with `--restart`. Alternatively, with a new id, a fresh `--run 7 --model <id>` writes a new slug directory.
