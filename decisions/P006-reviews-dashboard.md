---
id: P006
title: Reviews dashboard without a backend, plus one-click fix suggestions
date: 2026-10-17
kind: product
lane: chat
outcome: pending
call: too-early
judged_by: 
judged_on: 
repo_entry: none
pr: 
---

# P006 · Reviews dashboard + fix suggestions

## Decided (Sunny, 2026-10-17)
- **Web app = reviews dashboard, no backend.**
  - Pick a public repo that has the Action. PRs and TraceHound findings load from GitHub's public API in the browser.
  - Findings show as bug cards with code. Where a snapshot exists, they are pinned on the component graph.
  - Nobody can spend the Nebius credit through the site.
- **Fix button = one-click GitHub suggestion.**
  - One extra Super call per posted finding writes a minimal fix, posted as a suggestion block, so GitHub shows "Commit suggestion".
  - Product only: fixes are not benchmarked.
  - Implemented in packages/action (codex-06), so the frozen reviewer code is untouched.
- Rejected:
  - A full GitHub App with a server (8–10 days; security work; credit abuse until 15 Dec).
  - Tested fixes in a sandbox (minutes per finding; only works where tests exist).

## Why
Sunny: the current site looks like a repo visualizer. No visible bugs, no fix button, so it reads as slop. Judges may score from the video and site alone (R3).

## Open
- Sunny's list of the UI changes he wants.
- Which worker builds the dashboard: a separate Claude Code session in a third worktree, or Codex after codex-06.
