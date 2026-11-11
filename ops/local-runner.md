# Local runner: `scripts/run-tracehound.ts`

Runs TraceHound's reviewer locally on test-set cases, instead of a GitHub workflow. Source: [scripts/run-tracehound.ts](https://github.com/SunnyBagal/TraceHound/blob/main/scripts/run-tracehound.ts), documented in [docs/live-setup.md §3b](https://github.com/SunnyBagal/TraceHound/blob/main/docs/live-setup.md). Merged to main on 2026-11-10 as `d482d38`.

## Why it exists
Sunny ruled on 2026-11-10: no controller repo and no TraceHound review workflow on GitHub. TraceHound runs locally per case and is scored from local output, and posting on the PR is optional. So no Nebius secret exists on GitHub ([track-b-testset](../benchmark/track-b-testset.md)).

## What it does
```
node scripts/run-tracehound.ts --manifest <case manifest> --clone <git dir> [--ids …] [--execute]
  [--out-dir …] [--max-usd-per-case x] [--max-usd-run y] [--post --repo SunnyBagal/<fork> --pr-map …]
```
- **Pipeline:** it runs the Action's own pipeline (pool-verify-v1). `prepareReview` was split out of the Action's `runAction` so both share it.
- **Defaults:** it uses the Action's defaults: 3.5 Super VL, at most 15 comments, fixes and contracts on.
- **Commit pair:** the same one the fork PR shows, base → case head.
- **Output:** one JSON per case: findings in the neutral format, usage, and the ledger delta.

## Safety properties (each pinned by a test)
- **Dry run by default.** It prints a plan and makes no model call. `--execute` is paid, so it needs the spend freeze lifted, a `budget.md` line and artu1's approval.
- **Spend caps.** There is a per-case and a per-run cap. Each case's allowance is the per-case cap, cut to what is left under the global total, and split across the two reviewers. Spend goes through the normal client and ledger. The run stops before any case whose allowance would fall below the minimum useful amount ($0.07).
- **Never overwrites output.** An existing case file refuses the run unless you pass `--rerun`, which writes a new `.r2` file next to it.
- **Refuses incomplete clones.** A partial clone with missing objects is refused, and git never lazy-fetches.
- **Environment allowlist for git and gh** ([run-tracehound.ts:266-280](https://github.com/SunnyBagal/TraceHound/blob/main/scripts/run-tracehound.ts)).
  - **git** gets no service credentials, only runtime, locale and certificate variables.
  - **gh** keeps only its own GitHub auth: `GH_TOKEN` / `GITHUB_TOKEN` if set, or its config dir.
  - **Model keys and other services' secrets** are excluded from both.
  - **The model stages** run with the Action's environment plus the per-case caps. Failure messages are fixed text, so no secret or stderr is echoed.
- **`--post` is optional and guarded.**
  - Only SunnyBagal forks, with exact-argument `gh` calls.
  - The PR's base and head commits must match the case's pair. They are re-checked as the last read before the comment, so a ref that moves blocks the post.
  - A marker prevents duplicate comments, and an over-size body is refused.

## Review history
1. **Built:** on 2026-11-10 (`3ce5aa3`), together with `--trigger coderabbit|greptile|bugbot` in the test set's PR opener (≤ 10 per hour per tool and per tool-and-fork).
2. **codex round 1: NO-GO.** It found 3 P1 and 2 P2 issues: the env allowlist, base/head commit binding, the global-cap split, zero network in trigger dry runs, and test timing. All were fixed in `38c9167`, each with a regression test.
3. **codex re-review: NO-GO**, with two P2s left: the order of the re-check before posting, and a flaky posting test. Both were fixed in `eb41993`.
4. **codex round 2: GO** at `eb41993`. artu1 merged it, and the gates on main were green: action 302, analyzer 1027 (+6 skipped), viewer 215, typecheck.

## Status (2026-11-11)
No live paid run has happened, so it has spent $0. Its synthetic `--execute` tests (fake model, gh and git) pass. The spend freeze blocks paid runs. Held-out output must go to an `--out-dir` that the tuning agents never read.
