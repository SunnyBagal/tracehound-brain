# bunny overnight · task 3 · worktree-audit (report only)

## Started (2026-11-14T23:44:16Z)
Read-only audit of `git worktree list` in ~/Projects/TraceHound: branch or detached HEAD, merged into origin/main, last commit date, tracked/untracked changes; grouped safe / keep / unsure, with remove/prune commands for Sunny. Nothing removed, pruned or deleted. Data gathered while task 2's full-suite loop ran.

## Finished (2026-11-15T00:00:07Z)
Done (report only). 42 worktrees, not ~32: 31 safe to remove (merged into main, or for b2-followup patch-equivalent on main; untracked files only node_modules or a .vitest cache), 5 keep (main checkout with P1 running, b13-review-skipped waiting for P1 to close, pa-076g in progress, tonight's ui-01b and flaky-061a), 6 unsure for Sunny (ui-03-pricing closed unmerged, tracehound-ui ui/v2, one evaluation checkout (not named here), planner-b, codex/ts-protocol, codex's 076l review copy). Commands written, none run; no branch deleted. Full table in TraceHound run/out/bunny.overnight-2026-11-15.md.
