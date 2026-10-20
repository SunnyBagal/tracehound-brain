# codex-11: fix the Ultrareview #2 findings on PR #53 (int-01)

Tool: **ChatGPT Codex**, in `~/Projects/TraceHound-codex`. Use the **same Codex task that built int-01** if it is still open (it has the context); otherwise a new task. Paste everything below the line.

---

Ultrareview #2 reviewed PR #53 (`codex/int-01-contracts`, head `747a3a7`). The planning chat checked each finding against the code and confirmed all seven. Fix them on the **same branch**, so they land in #53. Make one commit per item, each with a test that fails before the fix. Keep `pnpm test` and `pnpm typecheck` green, and run them in a plain shell (the `GIT_*` env check prints nothing).

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` is empty; you are on `codex/int-01-contracts` at `747a3a7` (or later, if you already pushed to it).
- **Don't open** `SunnyBagal/linea-seeded`, `recall-seeded` or `tracehound-seeds`.

## Fixes

1. **Kill git and the analyzer cleanly** (`contracts.ts` ~69–92). A SIGKILL during `git fetch --unshallow` leaves `.git/shallow.lock`. The data-branch step runs later **in the same job and workspace** (composite `action.yml`, `data-branch: on`), and its fetch then fails silently under `continue-on-error`.
   - On timeout, send SIGTERM first and SIGKILL only after a 5 s grace. Git removes its own lock files on SIGTERM.
   - Start the base fetch only if at least 30 s of the 90 s budget are left; otherwise fail with `merge-base`.
   - If a fetch we started was still killed, and `.git/shallow.lock` exists afterwards, remove that file and log one line. Never touch any other lock.
   - Test: a fake exec that times out on fetch leaves no `shallow.lock`. The order SIGTERM → wait → SIGKILL is asserted.
2. **The analyzer gets the time that is left** (`contracts.ts:87`). Pass `--budget-s` = floor((90 s − elapsed − 10 s) / 1 s). If that is under 5, skip the analyzer and return `failed (timeout)`. The analyzer then stops on its own budget, and its `finally` closes the worktrees. Test: the argument shrinks after a slow fake merge-base.
3. **Keep keyed findings when the typed check is skipped** (`contracts.ts:45, 89–92`). `typed.skipped` is `"budget"` before any work whenever a side has more than 5,000 root files (`typed.ts:195`), so large monorepos always report failed today. Accept `typed.skipped` in `no-tsconfig | budget | error`.
   - Keep the keyed findings and the status `ok`, and add `typed: "complete" | "skipped-<reason>"` to the footer's `contracts` record.
   - Log one line: `contracts: typed check skipped (<reason>); key checks only`.
   - Tests for `budget` and `error` reports that carry keyed findings.
4. **Put the contracts section after the disclaimer** (`action.ts:175–179`). Today it sits before `These comments are model-generated and unverified.`, so `learn.ts bodyFindings` attaches the whole section to the last body-placed model finding. It also puts deterministic findings under the "unverified" line.
   - Order: findings… → omitted/suppressed lines → disclaimer → contracts section → hidden footer line. The footer must stay the last line, and `parseReviewFooter` reads only the last line.
   - Test: `bodyFindings` of a review with a body-placed model finding and a contracts section returns no contract text.
5. **Escape the reason text** (`contracts.ts:139`, and the `Contract:` inline body). GitHub strips `<User>`, so `Type 'Promise<User>'` renders as `Type 'Promise'`. A key such as `@team` pings that team.
   - Escape `& < > [ ] \` and backticks, and break `@` mentions with a zero-width space after `@`.
   - Test the generic-type message and an `@org/team` key.
6. **No checkout means no contracts, and no changed output** (`contracts.ts:71–81`; README says `review-diff-v1` needs no checkout). If `repoDir` is empty or `git -C <repoDir> rev-parse --git-dir` fails, the stage acts as `off`: no footer change, no section. Log `contracts: skipped (no checkout)`. Test: `review-diff-v1` with no checkout is byte-identical to `contracts: 'off'`.
7. **Docs: contract findings are always body findings.** The analyzer only reports files the PR didn't edit (`findings.ts:75`, `keyed.ts:218`), so the inline path, its cap and the ±3 dedupe can't trigger today.
   - Keep the code.
   - In `docs/action/README.md`, say so plainly and drop the claim that findings appear inline.
   - Also document the footer's `typed` field, the SIGTERM grace, and the "no checkout" rule.

## Not in this job
The duplicated merge-base/fetch code in `data.ts:197–206` (no credential-clearing flags there) goes to the backlog. Don't touch `data.ts`.

## Live check (required)
- Re-pin demo PR #6 in `SunnyBagal/tracehound-demo` (branch `demo/int-01-contracts`) to the new head sha. Push one empty commit to that branch to trigger one normal review (cents).
- Report the review URL, and confirm two things: the section now comes **after** the disclaimer, and the 4 dependents still show.
- Never merge demo PR #6.

## Rules
- Touch only `packages/action/src/` (`contracts.ts`, `action.ts`, `cli.ts`), `packages/action/test/`, and `docs/action/README.md`.
- Nothing in `packages/analyzer/`.
- Keep `cleanGitEnv` and `contractsEnv`. No new dependencies. No force-push, no merge. #53 stays a draft. Wait for CI and make one fix attempt if it's red.

End with: the head sha, one line per commit, the test counts, the byte-identical result for `contracts: 'off'` and for no checkout, the live-check URL and what it showed, and anything you couldn't do.
