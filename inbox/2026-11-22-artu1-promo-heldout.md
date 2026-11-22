# artu1 promo-heldout: HyperFrames bar chart replaces the Remotion promo (raw)

Sunny, 22 Nov: the Remotion race was "slop", and the line-chart redesign was rejected too. He pointed to his own October
HyperFrames benchmark video and asked for one like it. $0, no model call.

- **Build:** a copy of the October project with new data: segmented bars (one segment per bug, 0–20) growing from 0,
  official Greptile and CodeRabbit logos, the wolf for TraceHound. Greptile green with a "Most bugs caught" band;
  TraceHound red with a dashed line at 4. Frame 0 shows the full card (the X thumbnail). Text made larger and darker for
  phones. HyperFrames CLI pinned 0.9.80 (was 0.8.181); `check` passes, contrast 98/98 WCAG AA.
- **Data:** totals only, from `docs/bench/ts-heldout.md` (Greptile 12, CodeRabbit 10, TraceHound 4 of 20; 29 PRs, 20 real
  bugs; p = 0.008 vs Greptile, 0.11 vs CodeRabbit, not significant; medusa, payload, twenty, bullmq; free plans). No
  held-out folder opened. Not "winner": Greptile vs CodeRabbit was not tested.
- **Where:** TraceHound branch `promo/race-animation`, `promo/heldout/`, PR #79 @ `91b243c` (not merged); `promo/race/`
  removed in the same commit. MP4 `promo/heldout/renders/tracehound-heldout.mp4`, 1920×1080, 30 fps, 210 frames, 7.0 s,
  H.264 yuv420p, faststart, 254,744 bytes.
- **Checked by artu1:** frames at 0, 1.5, 3.0, 4.0 and 6.5 s. bunny's number check: `2026-11-22-bunny-promo-heldout.md`.
