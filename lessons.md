# Lessons

Process mistakes and near-misses, blameless. Each says what happened, what it cost, and what changed. Newest first.

### 2026-10-18 · A PR was merged while a fix prompt for it was being written (again)
- **What:** chat wrote codex-06c to add commits to #38's branch; Sunny merged #38 (and #39) minutes later. Same trap as 15 Oct.
- **Cost:** one rewritten prompt. Main is fine: the suggestions work, only demo #2's isn't committable.
- **Changed:** when the chat says "don't merge yet", NOW lists the PR as **HOLD** and the chat says it in the first line of its reply. A follow-up prompt for a draft PR is written against main on a new branch by default.

### 2026-10-18 · A 4-hour model batch ran as a background command inside the relay session
- **What:** the 062 brief (written by chat) didn't say how to launch the runs. The builder ran items 2–3 as one background shell command, and the session's 2-hour limit killed it at dev run 2, PR 30 of 40. The run was restarted as 2r (061a), so dev run 3 then hit the $4 cap at PR 16 of 40.
- **Cost:** $1.44 of Nebius with no score; the dev spread has 2 runs, not 3.
- **Changed:** any model batch over about 60 minutes is launched detached (`nohup … &` with its own log), and the brief says so. The session then polls `run.json`. The chat's progress commands count *files* (`find -type f`), not folders: `ls | wc -l` counted repo folders twice on 18 Oct.

### 2026-10-18 · The 062 brief's tag check peeled the tag
- **What:** step 0 expected `eval-freeze-2^{commit}` to print `acefcf9`, but that is the tag object; the commit is `b5cafde`. The builder handled it correctly and reported a deviation.
- **Changed:** check annotated tags with `git rev-parse TAG` (object) **and** `TAG^{commit}` (target), and name both.

### 2026-10-17 · Codex got codex-06 before codex-05 existed
- **What:** two Codex prompts went out the same day, and 06 was pasted first. Its step 0 ("the pnpm pin is merged") caught it and stopped at $0.
- **Kept:** a step 0 that checks the previous job has merged. It worked.
- **Changed:** NOW lists the next prompt per lane, and the chat names the exact file to paste each time.

### 2026-10-17 · A risk named on day one was never tested, and the demo was built on it
- **What:** on 15 Oct the chat named `package_json_file: ${{ github.action_path }}/../../../package.json` as "the most likely thing to break on a real run". No live smoke test was scheduled. #25, #28 and #30 merged with unit tests only. Then all 7 runs in the dogfood and demo setup (codex-04) failed in that step: "No pnpm version is specified", because pnpm resolves the path against the target repo.
- **Cost:** one Codex cycle and a day of the demo schedule. $0 of Nebius, since it failed before any model call.
- **Changed:** a named risk gets a test that would catch it before anything is built on top. The fix PR (codex-05) adds a test that fails if `package_json_file` returns, and checks the version against the root `packageManager`. Every Action change now gets a live run on the demo repo, pinned to the branch sha, before merge.

### 2026-10-15 · CI green does not mean the lockfile is current
- **What:** #25 added `packages/action` without a lockfile importer. CI's `--frozen-lockfile` passed, but relay's local `pnpm install` during 058 rewrote `pnpm-lock.yaml`. Relay restored it by hand.
- **Cost:** none yet. The next relay step 0 would fail on a dirty tree.
- **Changed:** a lockfile commit goes with the PR that adds a workspace package. The chat's earlier lockfile worry was right about the drift and wrong about CI.

### 2026-10-15 · An untracked "AGENTS.md" was really a copy of CLAUDE.md
- **What:** relay's checkout had an untracked `AGENTS.md`. Once the real AGENTS.md was merged (#25), step 0 would have failed. `cmp` showed the local file differed. Its blob was `f9fab6e`, CLAUDE.md as of `f610efa`.
- **Cost:** none, because it was moved, not deleted.
- **Changed:** an unknown untracked file is moved out of the repo and compared before anything is deleted.

### 2026-10-15 · A fix prompt aimed at a branch that was merged ten minutes later
- **What:** `codex-02-pr25-fixes.md` added commits to `codex/github-action`; #25 was merged before it ran.
- **Cost:** one rewritten prompt.
- **Changed:** when a merge is likely before a fix prompt runs, the fix prompt is written against main on a new branch.

### 2026-10-15 · Chat predicted red CI on #25 (lockfile); CI was green
- **What:** the new workspace package `packages/action` had no lockfile entry. The chat called that a "probable" `--frozen-lockfile` failure. pnpm accepts a package with no dependencies that is missing from the lockfile.
- **Cost:** a wasted step in a prompt, later removed.
- **Changed:** check the Actions page (WebFetch of `/actions/workflows/ci.yml`) before predicting CI.

### 2026-10-15 · 057's budget cut the variable under test first
- **What:** the 057 brief (written by chat) set a 40k-character budget and, when over, dropped references before anything else. References to callers were the point of the experiment. They reached the model in 12 of 48 chunks, so 057 mostly measured "changed files near the hunks".
- **Cost:** one $0.86 run that could not answer its own question.
- **Changed:** a budget's cut order protects whatever is being tested. 058's per-finding context drops references last-first only after the file window has been shrunk.

### 2026-10-15 · A single commit made "prompt frozen before the run" unverifiable
- **What:** relay's reviewer could not check 057's freeze claim, because everything landed in one commit.
- **Changed:** 058 pushes a freeze commit (rules and prompt) before the first model call.

### 2026-10-15 · Model-reported confidence did not separate true from false findings (056)
- **What:** 6 of 32 TPs got confidence 1. The pre-set threshold rule kept everything.
- **Changed:** 058 asks for a keep/drop verdict with context instead of a number.

### Before 2026-10-15 · Four relay cycles lost to step-0 stops
- **What:** unmerged PR, $0 Nebius cap, a stale brief rerun, stale local main.
- **Changed:** the pre-start checklist in [NOW](NOW.md); step 0 uses `git fetch origin main:main` (no plus), then `git switch main`.
