# 071b: fold Ultra dev run 11 into PR #63 and fix Devin's 4 findings

Tool: **relay**, i.e. Sunny's Claude Code (account A), builder plus reviewer. Folder `~/Projects/TraceHound`. Start it from `~/Projects/tracehound-ops` in a **plain terminal**:

```
cd ~/Projects/tracehound-ops && caffeinate -i bun src/relay.ts start tracehound briefs/071b-run11-and-devin-fixes.md
```

**Sunny, before starting:**
- `runs/071-chain.log` ends with `chain done` (you pasted it to the chat).
- Copy this file to `~/Projects/tracehound-ops/briefs/071b-run11-and-devin-fixes.md`.
- `nebiusCapUsd` in `ops/lanes.json` = **0** (this job makes no model calls).

Follow-up to decision **071** (no new decision number). Nebius **$0**: no model calls of any kind. PR **#63** (`build/071-dev-ultra`, head `6bd72e1`, draft). Target: PR updated within about 1 hour.

## Context
- #63 reports Ultra dev run 10 alone. Run 11 ran unattended and its chain script wrote `runs/071-report-final.txt` and `chain done` in `runs/071-chain.log`.
- **Devin reviewed #63 and found 4 bugs.** Chat checked them against the code; all are real, and none changes run 10's numbers:
  1. **Baseline mismatch does not stop** (`scripts/bench/dev-ultra.ts:~177`): it logs "DIFFERS BY MORE THAN 0.1: STOP" and keeps printing tables.
  2. **Token Factory Ultra calls bypass spending caps** (`config/prices.json:9`): `nvidia/nemotron-3-ultra-550b-a55b` is priced 0/0 whatever the route, so a request with that id that goes to Token Factory (flag unset) reserves $0.
  3. **Verifier truncations disappear from run counts** (`dev-ultra.ts:~160`). **Bugbot found the cause (same PR):** `ledgerWindow` treats a call as a verifier call when `purpose` starts with `verify`, but verify-ctx ledgers as `"review"` (`VERIFY_CTX_PURPOSE`), so every call is checked against the reviewer's 32,000 cap and verifier truncations at 16,000 are never counted. The fix can't use `purpose`; tell verifier calls apart another way (e.g. the attempt records' verdicts, or the request's `max_tokens` if the ledger keeps it) and don't change any ledger format.
  4. **Ultra cache entries cross provider routes** (`packages/analyzer/src/llm/client.ts:~263`, `cache.ts`): the cache key is model + request, not route.
- A relay session (038) was stopped mid-way on 24 Oct. It may have left `docs/prompts/071-followup-run11.md` untracked.

## Step 0 (if any check fails, STOP AND REPORT)
- `git fetch origin`. `git status --porcelain`: if it prints only `?? docs/prompts/071-followup-run11.md`, move that file to `runs/071-followup-run11.stale.md` and continue. Anything else printed: STOP.
- Switch to `build/071-dev-ultra`; it equals `origin/build/071-dev-ultra` at `6bd72e1`.
- `runs/071-chain.log` contains `chain done`. Read it, `runs/071-dev11.log` and `runs/071-report-final.txt`. STOP if run 11's exit isn't 0, its status isn't complete, or a `baseline check` line says DIFFERS. More than 10 of 40 PRs with failed chunks is a finding, not a stop.
- Read Devin's 4 review comments on PR #63 (`gh pr view 63 --comments`, and `gh api repos/SunnyBagal/TraceHound/pulls/63/comments`).
- The `GIT_*` env check prints nothing.

## Steps (one commit each; gates as their own commands; messages `area: what changed`)
1. Save this brief verbatim as `docs/prompts/071b-run11-and-devin-fixes.md`. Commit.
2. **Fix 2 (caps) and fix 4 (cache), in the client.** The $0 price and the NVIDIA cache namespace apply **only when the request is actually routed to the NVIDIA API** (flag set and that exact id). With the flag unset, the lowercase Ultra id prices at `placeholderFallback` like any unknown id, and its cache key is exactly what it is today. Token Factory requests, prices and cache keys for every other model stay byte-identical (existing tests stay green; add tests for both fixes: unflagged lowercase id reserves the fallback estimate; flagged and unflagged requests never share a cache entry). Keep `prices.json`'s entry if the client needs it, but its `_readme` sentence must say it's honoured only on the NVIDIA route. Commit.
3. **Fix 1 and fix 3, in `scripts/bench/dev-ultra.ts`.** Fix 1: a baseline mismatch prints the line, sets a non-zero exit code and prints no tables. Fix 3: reproduce Devin's claim first; if real, fix it so verifier truncations are counted and shown; if not, write one sentence why in the PR reply. **The scoring and the bootstrap rule must not change**: add a test that the report's tables for run 10 are byte-identical before and after (use the saved `runs/071-report-*.txt` as the expected output if the records aren't available to tests; otherwise a fixture). Commit.
4. **Regenerate the report** with `node scripts/bench/dev-ultra.ts > runs/071-report-final-2.txt` (no model calls). Its tables must equal `runs/071-report-final.txt` except the counts fix 3 adds. If anything else differs, STOP.
5. **Update `docs/bench/dev-ultra.md` from that report only:** runs 10 and 11 per run plus the 2-run Ultra mean, v1 and x1, both rows, next to both baselines; the paired-bootstrap table with both Ultra runs (10,000 resamples, seed 71); run 11's operations (failed chunks, truncations incl. the verifier's, 429s, timeouts and reruns, wall time, tokens per PR); run 11's Nebius judge spend. Say "Ultra is better/worse" only where an interval excludes 0; otherwise "no interval-backed difference". Replace "It was the most reliable model on the pipeline" with counts only. Fix the commit reference (`0dec002`). One sentence on how the baseline side of the bootstrap is formed (mean of per-run F on each resample). Remove the "fallback in force" wording, keeping one line that run 10 was first reported alone. A short "Fixed after review (Devin)" list with the 4 findings. Commit.
6. **Decision 071** (the last entry in `docs/decisions.md`, unmerged in this PR): update its result to both runs; change nothing above it. **`FEEDBACK.md`**: update only the 24 Oct job-071 entry with run 11's judge spend and NVIDIA API counts, kept separate. Commit.
7. `pnpm test` and `pnpm typecheck` as separate commands; read each. Timeouts-only failures: rerun those files with `--testTimeout 30000` and record it.
8. Push. Reply on each of Devin's 4 threads with the commit that fixes it (or the one-sentence reason). Update PR #63's body for both runs and the fixes. **Mark it ready for review** (`gh pr ready 63`), so Devin and the dogfood check run. Wait for CI; one fix attempt if red. Never merge.

## Rules
- **$0, no model calls.** Never print, log or commit `NVIDIA_API_KEY` or `NEBIUS_API_KEY`.
- **Cal.com is blind**: nothing under `~/Projects/tracehound-bench/test/`; no `--set cal_dot_com`. Nothing of ReviewBench, `linea-seeded`, `recall-seeded`, `tracehound-seeds`.
- May touch: `packages/analyzer/src/llm/` and its tests, `config/prices.json` (the Ultra entry and its `_readme` sentence only), `scripts/bench/dev-ultra.ts` and its tests, `docs/bench/dev-ultra.md`, the `## 071` entry of `docs/decisions.md`, the 071 entry of `FEEDBACK.md`, `docs/prompts/071b-run11-and-devin-fixes.md`, `runs/`.
- Don't touch: `docs/bench/dev-ultra-plan.md`, `packages/action/`, `packages/analyzer/src/review/`, `scripts/reviewbench/`, `.github/`, `viewer/`, `README.md`, `docs/bench/results.md`, `AGENTS.md`, `pnpm-lock.yaml`, `.env`, `~/Projects/tracehound-ops/`.
- No tags, no history rewriting, no destructive git. Stage by explicit path.

## STOP AND REPORT
Final message = the report: step 0 results (incl. what was in the tree); run 11's exit/status/baseline lines; the updated tables (per run and mean, v1 and x1, both rows, both baselines); intervals and which exclude 0; run 11's operations; judge spend (run 11 and 071 total); Ultra token totals; each Devin finding → fix commit or reason; diff stat; gate results; PR #63 head sha and CI state.

Also write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-27-sunny-071b.md` (rules in `inbox/README.md`: only that file; then `git pull --rebase`, commit and push).
