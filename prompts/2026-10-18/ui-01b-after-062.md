# ui-01b: step 9 after 062 (paste into the SAME Codex Sol 6.1 task that made PR #35)

Tool: ChatGPT Codex, GPT Sol 6.1. Folder: `~/Projects/TraceHound-ui` (worktree). Paste only once Sunny has checked 062's PR in the planning chat.

---

062 is done.

PR #35 (`ui/viewer-round-1`) is already merged into main with a merge commit, so do not add commits to that branch.

Step 0:
1. `git fetch origin main:main` (no plus), then `git switch -c ui/round-1-summaries main`.
2. Confirm `git log --oneline -1 main` is the merge of #35 or later, and that `git status --porcelain` prints nothing.
3. If either check fails, stop and report. Do not call Nebius.

Then run step 9 from `ui-01-codex.md` exactly as written there, with a $0.20 cap: Nemotron Super one-line summaries for the Recall and tracehound-demo snapshots at their current main commits.

- Write the summaries into the snapshot data with their provenance marked as model-written (Nemotron 3 Super). Heuristic subtitles stay as the fallback.
- Respect the existing `summary: false` overrides.
- Add the FEEDBACK.md entry step 9 asks for.
- Use small commits, each with green gates.
- Open a PR to main and do not merge it. In the PR body, list every summary as `component id → summary`, plus the spend from the ledger.
