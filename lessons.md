# Lessons

Process mistakes and near-misses, blameless. Each says what happened, what it cost, and what changed. Newest first.

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
