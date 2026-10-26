# codex-14: levers v2 — offline replay on saved dev records ($0, report only)

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Paste everything below the line.

---

TraceHound's PR reviewer is frozen (`review-freeze-2`, pool-verify-v1 on Nemotron 3.5 Super). This job measures four cheap ideas **offline**, by replaying saved dev runs and their saved judge matches. **No model call, no network call, $0.** Nothing is adopted: the result is a report for the "what's next" section.

Decision number: none (report `levers-v2`). Branch `codex/levers-v2`. Draft PR. Target: within about 3 hours.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` is empty; branch `codex/levers-v2` from `origin/main` (contains `36de717`).
- `pnpm install --frozen-lockfile`; `pnpm test` and `pnpm typecheck` green (timeouts-only failures: rerun those files with `--testTimeout 30000`).
- The dev records exist under `~/Projects/tracehound-bench/reviews/dev-repeat/` (3 Super runs 1, 2r, 4, 5, 6; 3.5 runs 7r, 8; Ultra runs 10, 11 if present) together with their judge outputs (summaries and per-comment matches for bench-judge-v1 and x1). Find the layout by reading `scripts/bench/run-test-set.ts`, `scripts/bench/vote.ts`, `scripts/bench/v3-results.ts` and `packages/analyzer/src/bench/score.ts`.
- **Never** open anything under `~/Projects/tracehound-bench/test/` (Cal.com is the blind set), ReviewBench data, `linea-seeded`, `recall-seeded` or `tracehound-seeds`. Dev golden comments may be read by scripts.

## How to replay without a judge call
`scripts/bench/v3-results.ts` shows the method: take a run's judged comments, drop some, and rescore the rest with the scorer's own `scorePull` on the saved matches. A removed comment's duplicate can become the TP; that's correct. Reuse it; don't write a new scorer.

## Step 1: pre-register (commit and push before computing any result)
Write `docs/bench/levers-v2-plan.md` with exactly these four replays, their parameters and what will be reported. No parameter may be added after you've seen a result.
1. **Per-PR cap (top-k).** For each PR keep only k findings, k ∈ {1, 2, 3, 4, 5}. Ranking (no model; the findings carry no severity or confidence field): **(a) agreement first** — findings that agree with a finding from the other generator (`scripts/bench/agree-rule.ts`) rank first, then review-diff-v1 findings, then review-ctx-only, each in output order; **(b) output order** as a control. Applied to pool-verify-v1 and pool-raw.
2. **Verifier on/off.** pool-raw vs pool-verify-v1 per run for 3 Super, 3.5 and Ultra (if records exist), v1 and x1 (this is a table of existing numbers plus a paired bootstrap of the difference: 10,000 resamples, seed 72, PR level).
3. **Cheaper vote.** keep-if-≥2-of-3 across runs, on 3 Super (runs 4, 5, 6, and 1, 2r, 4) using `vote.ts`'s matching rule unchanged. Report next to each run's own F and the 3-run mean.
4. **Where the misses are.** For pool-verify-v1 on 3.5 (runs 7r, 8): the golden comments missed in both runs, counted by repo and by golden severity. Counts only (no comment text in the repo or the PR).

## Step 2: build
- `scripts/bench/levers-v2.ts` (one script, prints markdown tables; `--json` for every number), tests for the replay helpers in `scripts/bench/levers-v2.test.ts` (fixtures, no records needed). Reuse `score.ts`, `agree-rule.ts`, `vote.ts` helpers; don't change them.
- Run it. Write `docs/bench/levers-v2.md`: the four tables under both judges, n beside every total, the bootstrap intervals, and a plain answer per lever. "Better" only where a 95% interval excludes 0. State the caveat once at the top: **the 40 dev PRs have been used for 17 earlier decisions; any gain here is a hypothesis, not a result, and none is adopted.**

## Step 3: PR
- `pnpm test`, `pnpm typecheck` as their own commands; then commit by explicit path; push; open a **draft** PR titled `levers v2: offline replay on dev (report only, $0)`. Never merge.

## Rules
- May touch: `scripts/bench/levers-v2.ts`, `scripts/bench/levers-v2.test.ts`, `docs/bench/levers-v2-plan.md`, `docs/bench/levers-v2.md`. Nothing else.
- Don't touch: `packages/`, `config/`, `.github/`, `viewer/`, `README.md`, `docs/bench/results.md`, `docs/decisions.md`, `pnpm-lock.yaml`, `.env`, other scripts.
- No comment text, titles, golden text or code from the records in any committed file or the PR: counts only.
- No tags, no destructive git.

## Report (paste as your final message)
Step 0 results; plan commit sha and time vs the first computed result; the four tables (v1 and x1); intervals; the plain answer per lever; diff stat; gate results; PR number and head sha.
