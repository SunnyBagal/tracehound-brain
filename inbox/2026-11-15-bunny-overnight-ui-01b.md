# bunny overnight · task 1 · ui-01 follow-up (ui-01b-inspector-bounds)

## Started (2026-11-14T23:37:44Z)
ui-01 already exists and was merged to main as PR #75 (9bf04b2, on Sunny's ruling, with codex-ui NO-GO on 31ae6f5 for two P2s in viewer/lib/inspector-width.ts kept open as follow-ups). The five requested items are in main. This task continues ui-01 by fixing the two open P2s on a new branch ui-01b-inspector-bounds off main (4fce41e). $0, no model calls, no merge.

## Finished (2026-11-14T23:41:54Z)
Partial by design: the five ui-01 items were already merged (PR #75); this run fixed the two open codex-ui P2s.
- PR #77 (open, not merged), branch ui-01b-inspector-bounds, head 4fa56e0 on main 4fce41e; 5 files (3 viewer source, 2 tests).
- (a) default inspector width kept inside the resize bounds at 768-859px; (b) the expanded PR list narrows the open inspector (1280px after End: 900 -> 652; canvas ~256px instead of ~8px); stored width kept.
- Viewer 228 tests passed, typecheck and build clean. Headless-Chrome before/after numbers in the PR. $0.
- Waiting on: codex-ui review, Sunny's merge.
