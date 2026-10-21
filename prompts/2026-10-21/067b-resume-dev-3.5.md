# 067b: resume 067 (Nemotron 3.5 Super on dev). 3.5 is callable now

Tool: **relay**, i.e. Sunny's Claude Code (account A), builder plus reviewer. Folder `~/Projects/TraceHound`. Start it from `~/Projects/tracehound-ops`:

```
caffeinate -i bun src/relay.ts start tracehound briefs/067b-resume-dev-3.5.md
```

**Sunny, before starting:**
- Copy this file to `~/Projects/tracehound-ops/briefs/067b-resume-dev-3.5.md`.
- Open `~/Projects/tracehound-ops/ops/lanes.json` and set `nebiusCapUsd` to **8**. Relay's facts showed $0 last time; check it really says 8.

Decision 067, continued. Nebius cap: **$8** for runs 7–9 (the runner's `CAP_067_USD`). Ledger about $25.82; the total cap is $45. **Deadline: the verdict is written by 23 Oct 23:59 IST** (freeze-2 is 24 Oct).

## What changed since 067 stopped
- **Why run 7 got 404s:** Nebius serves `nvidia/Nemotron-3.5-Super-VL` only from **us-north1**. us-central1 lists it in `/v1/models` but answers 404, and 3 Super exists only on us-central1. **The model id was right.**
- **#55 (main `da13585`) fixes it in the client:** `baseUrlFor` in `packages/analyzer/src/llm/client.ts` sends that exact id to `https://api.tokenfactory.us-north1.nebius.com/v1`, and every other model to the configured URL. The judges (3 Super, Qwen) stay on us-central1. No prompt, cache key or price changed. `config/prices.json` already has the 3.5 key.
- **The ReviewBench smoke test on 3.5 (21 Oct)** reached the model: one small PR was ok. On a large PR the reviewer returned **invalid JSON twice**, and that PR failed (`review: failed chunks (first: the reply is not valid JSON; after one retry: the reply is not valid JSON)`), costing $0.096 against $0.042 on 3 Super. The likely cause is reasoning tokens using up `max_tokens` (32,000) before the JSON. That isn't verified, because chunk records keep no `finish_reason`. **This is model behaviour under the frozen settings, not an infrastructure failure.**

## Step 0 (if any check fails, STOP AND REPORT)
- In `~/Projects/TraceHound`: `git fetch origin`; `git status --porcelain` prints nothing.
- `git switch build/067-dev-3.5`; HEAD is `cf5b70b`.
- `origin/main` contains `da13585`.
- The `GIT_*` env check prints nothing.
- `run-7-nvidia-nemotron-3-5-super-vl/run.json` still says status `running` with nothing scored (void under 061a).
- The ledger is $25.82 or less.

## Steps (one commit each; run `pnpm test` and `pnpm typecheck` as their own commands, read the result, then commit separately)
1. **Bring in the fix.** `git merge --no-ff origin/main` into `build/067-dev-3.5`: a merge commit, never a rebase or force-push.
   - If there's a conflict outside `scripts/bench/` or `docs/bench/`, STOP and report.
   - Tests and typecheck must be green.
   - Push.
2. **Amendment commit to `docs/bench/dev-3.5-plan.md` (push it before any model call).** Append a section, `Amendment 1 (21 Oct, before any successful 3.5 call)`, that says exactly this:
   - The model id is unchanged. The 404s were a region problem, fixed in the client by #55 (main `da13585`), which this branch merged at `<merge sha>`.
   - The original run 7 is void (an infrastructure failure, $0). It is restarted once under 061a with `--restart` into `run-7r-…`, which stands for run 7. Runs 8 and 9 follow.
   - **New stop rule before run 8** (it replaces the brief's "any PR has failed chunks"). Stop and report if any of these holds:
     - every PR got 0 findings;
     - any failure string is a request error (HTTP 4xx/5xx, a budget refusal);
     - more than 10 of the 40 PRs have a failed reviewer chunk.

     Otherwise continue.
   - Failed chunks caused by invalid or truncated replies are **counted as the candidate's result**: those PRs are scored exactly as the scoring scripts score any PR whose reviewer failed. That is the same rule the baseline runs were scored with. State that rule in one sentence, and cite the file and line where it lives.
   - The adoption rule and thresholds are **unchanged**: v1 mean F ≥ 45.28, v1 mean P ≥ 40.52, x1 mean F ≥ 51.29, plus the 2-run fallback if the cap stops a run.
3. **Run 7r, then 8 and 9**, with `--model nvidia/Nemotron-3.5-Super-VL`. Detach each one and poll `run.json` as before.
   - **Before run 7r starts:** make one request through the client (any stage, one small dev PR is fine) and confirm it went to us-north1 and succeeded. If it 404s, STOP.
   - **Before run 8:** apply the amended stop rule. Record, per run: failed-chunk PRs, their failure strings (first 120 characters), and completion tokens on failed chunks if the records hold them.
4. **Score and write `docs/bench/dev-3.5.md`** as the original brief says:
   - per-run and mean P/R/F under v1 and x1 against the baseline;
   - the bootstrap (10,000 resamples, seed 67);
   - tokens and cost per PR for 3.5 vs 3 Super.

   Plus one table: failed-chunk PRs per run and the failure strings.

   State the verdict plainly. Append decision 067 to `docs/decisions.md`, and update the FEEDBACK.md entry: region, then the JSON failures with counts.
5. Push. Open the PR to main **as a draft**, titled `067: Nemotron 3.5 Super on dev (decision 067)`. Wait for CI; make one fix attempt if it's red. Never merge.

## Rules (unchanged from 067)
- **Cal.com is blind**: nothing under `~/Projects/tracehound-bench/test/`, and no `--set cal_dot_com`.
- No ReviewBench data, and nothing under ReviewBench's `golden/`.
- Don't open `linea-seeded`, `recall-seeded` or `tracehound-seeds`.
- May touch: `scripts/bench/` and its tests (including the test files under `packages/analyzer/test/` that 067 already uses), `docs/bench/`, `docs/decisions.md` (append only), `docs/prompts/067-dev-nemotron-3.5.md`, `FEEDBACK.md`, and `runs/`.
- Don't touch: `packages/*/src`, `config/`, `.github/`, `viewer/`, `AGENTS.md`, `pnpm-lock.yaml`, or anything in `~/Projects/tracehound-ops/`.
- No prompt or `max_tokens` changes. No tag operations, no destructive git. Stage only by explicit path.

## STOP AND REPORT
Make your final message the report:
- step 0 results;
- the merge sha;
- the amendment commit sha and its time against the first 3.5 call;
- the table;
- the failed-chunk table;
- the bootstrap interval;
- spend;
- the verdict;
- the PR number and head sha.

Also write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-2x-sunny-067b.md` (rules in `inbox/README.md`: only that file; `git pull --rebase`, commit, push).
