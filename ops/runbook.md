# Runbook: how work moves

## Roles

| Who | Job | Never |
|---|---|---|
| **Chat** (Claude, claude.ai) | Plans; writes every brief and prompt as a file; checks every PR from GitHub before Sunny merges; keeps this vault current | Reads Cal.com content; opens Recall PRs #1/#2 |
| **Relay** (Claude Code builder + reviewer) | Benchmark lane, one job at a time, in `~/Projects/TraceHound`; branches `build/*`, `docs/*`; posts STOP AND REPORT as a PR comment | Touches Codex paths or AGENTS.md; merges its own PR. (Relay's reviewer can't run `gh`, so it can't see CI) |
| **Codex** (ChatGPT Plus) | Product lane in `~/Projects/TraceHound-codex`; branches `codex/*`; owns `.github/actions/`, `packages/action/`, `docs/action/`, `docs/demo/`. Also automatic PR review on GitHub (rules in AGENTS.md `## Code Review Rules`) | Touches benchmark data or relay paths |
| **Grok** (Premium) | No code. Audits each results doc after merge; X posts; video script. Fresh chat each time | Sees Cal.com data or the chat's discussion |
| **UI lane** (Codex GPT Sol 6.1 since 17 Oct; was planned as a Claude Code session) | `viewer/`, `snapshots/`, `changesets/`, `impacts/`, `configs/`, `docs/ui/` in worktree `~/Projects/TraceHound-ui`, branches `ui/*` | Touches `packages/analyzer/src/` (frozen), bench, action or Codex paths |
| **Claude cloud** (Claude Code on the web, claude.ai/code; $96 credit, expires **5 Nov**; added 18 Oct) | Parallel research, audit and spike jobs on `SunnyBagal/TraceHound`, one fresh session per prompt (`cc-NN`), draft PRs only | Has no Nebius key and makes no model calls through our code; never fetches Cal.com content; never merges |
| **Sunny** | Merges; runs relay, Codex, the UI session and cloud sessions; decides | — |

**Grok kill test:** every audit finding must cite a file and line that exist. One invented citation means Grok is dropped and the audit is rerun in a fresh ChatGPT chat ([P003](../decisions/P003-grok-kill-test.md)).

## Starting a relay job

1. The PR relay waits on is merged.
2. `git -C ~/Projects/TraceHound status --porcelain` prints nothing. **No output is the good result.**
3. `nebiusCapUsd` in `~/Projects/tracehound-ops/ops/lanes.json` (line 11) matches the brief. It is currently 3. At 0, any Nebius spend stops the run.
4. Never rerun a brief whose PR is already open.
5. Move the brief, then start:
   ```
   mv "$(ls -t ~/Downloads/NNN-name*.md | head -1)" ~/Projects/tracehound-ops/briefs/NNN-name.md
   cd ~/Projects/tracehound-ops && caffeinate -i bun src/relay.ts start tracehound briefs/NNN-name.md
   ```
   The Mac renames repeat downloads to `name (1).md`; `ls -t … | head -1` picks the newest.
6. When the PR opens, say "check #N" in the chat. Paste relay output only if it stopped before opening a PR.

## Every brief contains

- Step 0: fetch; the allowed status lines; if not on main, `git fetch origin main:main` (no plus), then `git switch main`; HEAD = origin/main; decision-number checks; inputs exist.
- Pre-registered rules and a **freeze commit pushed before the first model call** (from 058 on).
- The GIT_* env check; `pnpm test` and `pnpm typecheck` in a plain shell; stage by explicit path; open a PR and never merge it; one fix attempt on red CI.
- Cal.com blind; counts only in messages; no destructive git; a spend cap.
- A STOP AND REPORT list, posted as a PR comment.
- **Small real commits** (Sunny, 17 Oct): one commit per logical step, each passing its tests; messages `area: what changed`. No empty, "wip" or padding commits. Merge PRs with **Create a merge commit** so every commit lands on main (a squash collapses them to one).

## How the chat checks a PR

1. `git ls-remote` and fetch `refs/pull/N/head`. Check the base is current main and the file list is inside the lane's paths.
2. Read the diff of code and docs; recompute every table's P/R/F from its counts.
3. For experiments: freeze commit time before the first call; `git diff <freeze> HEAD` shows no prompt or rule change; the adoption rule was applied as written.
4. CI: WebFetch of the PR's checks page. The Actions list page is often stale; ask Sunny to look if the result isn't shown.
5. Then update this vault: [NOW](../NOW.md), [prs](../prs.md), [results](../results.md), [timeline](../timeline.md), the decision note, [strengths and gaps](../strengths-and-gaps.md), [lessons](../lessons.md) if a process mistake happened. Regenerate [digest](../decisions/digest.md).

## Known traps

- **Untracked files in relay's checkout** fail step 0. Move unknown files out and compare before deleting anything ([lessons](../lessons.md)).
- **Lockfile drift:** a PR that adds a workspace package must commit its lockfile importer. CI's `--frozen-lockfile` won't catch it.
- **A fix prompt for an open PR** goes stale if the PR merges first. Write fixes against main on a new branch.
- **Codex automatic review** posted nothing on #27/#28. Check its Review trigger, or comment `@codex review`.
