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

## UI feedback (Sunny, 2026-10-17, from screenshots)
- The "toy cart" (a repair-harness test fixture) shows in the public repo switcher. Remove it.
- Recall's orphan warnings point at deleted files, because the snapshot @ `5d2165a` is stale. Regenerate it at main.
- The SHA copy chip is unclear. Replace it with "main @ sha · date · N behind".
- Canvas nodes should expand to show their contents (routes on the API node, and so on).
- The inspector's Summary and Stack are cramped. Routes and files should expand to show code inline, keeping the GitHub link.
- **Later:** a live execution-flow trace (login → … → final execution, each node lit).
- Dashboard flow confirmed: Landing → pick repo → PR list with bug counts → PR page (bug cards, fix diff, "Commit fix on GitHub", affected component lit) → Benchmark page.
- New: open PRs overlaid on the graph showing affected nodes; merged PRs leave the overlay and stay in a history log.

## Plan
- **ui-01** (2026-10-17): the cleanup, fresh snapshots (plus a tracehound-demo snapshot), expandable nodes, readable inspector, inline code.
- **ui-02** (after codex-06 merges): reviews dashboard, PR overlay plus history, benchmark page.
- **Worker:** a plain Claude Code session in the worktree ~/Projects/TraceHound-ui, branches `ui/*`, as a third lane.
