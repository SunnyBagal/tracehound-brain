# artu1 promo-race: race animation for the result post (raw)

Sunny's task, 22 Nov: a ~9 s looping MP4 for the X post about the held-out result. Built with Remotion in `promo/race/`
on TraceHound branch `promo/race-animation`, PR #79 (not merged). It is its own pnpm root, outside the workspace and CI.
$0, no model call.

- **Data:** totals only, from `docs/bench/ts-heldout.md`: Greptile 12, CodeRabbit 10, TraceHound 4 of 20 bugs; 29 PRs;
  medusa, payload, twenty, bullmq. Sunny chose this over a per-PR race. A PR-by-PR order would let anyone match steps to
  the public fork PRs, so the racers move in even one-bug ticks and no held-out folder was opened.
- **Commits:** `b61b85f` first render; `8cd38b3` applies bunny's four review fixes ("one run" in the footer, a larger and
  brighter footer, "Nobody finished." held about 0.8 s, visible bar tracks).
- **Output:** `promo/race/renders/tracehound-race.mp4`, 1920×1080, 30 fps, H.264 High, yuv420p, faststart, 270 frames,
  9.0 s, 539,180 bytes; stills `tracehound-race-first.png` (frame 0) and `tracehound-race-scoreboard.png` (frame 240).
  Logos: official files dropped into `promo/race/assets/` replace the text labels on re-render. Nothing draws a logo.
- **Checked by artu1:** frames at 0.5, 3.5, 6.5, 7.0 and 8.0 s at 1920 px and 640 px; ffprobe spec; moov before mdat.
- **Reviews:** bunny numbers PASS; grok stopped partway through its review on its weekly usage limit; bunny stood in for
  the honesty and readability review: GO with four non-blocking fixes, all applied; bunny re-check on `8cd38b3`: GO (`2026-11-22-bunny-promo-recheck.md`).
