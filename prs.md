# PR log

Every merged PR on TraceHound main, from merge commits. Update after each merge.

| PR | Merged | Merge sha | Branch | Lane | Title |
|---|---|---|---|---|---|
| [#1](https://github.com/SunnyBagal/TraceHound/pull/1) | 2026-09-30 | `cfe5038` | `ui/canvas-polish` | UI | Viewer: navy canvas, fact-backed tech icons, Railway-style inspector with history navigation |
| [#2](https://github.com/SunnyBagal/TraceHound/pull/2) | 2026-10-01 | `da916f1` | `ui/canvas-polish-2` | UI | UI-2: canvas polish (decision 031) |
| [#3](https://github.com/SunnyBagal/TraceHound/pull/3) | 2026-10-03 | `dd1d5ac` | `ui/change-view` | UI | Change view (?changes=) and repo gallery (?repo=); site opens on Recall |
| [#4](https://github.com/SunnyBagal/TraceHound/pull/4) | 2026-10-03 | `07e0147` | `harness/recall` | relay | Repair harness on Recall: repo profiles, baseline-matched typecheck gate, smoke task (decision 041) |
| [#5](https://github.com/SunnyBagal/TraceHound/pull/5) | 2026-10-03 | `72bc39f` | `analyzer/queue-di` | relay | BullMQ queues through dependency injection (bullmq-queues@0.2, analyzer 0.10.0, decision 042) |
| [#6](https://github.com/SunnyBagal/TraceHound/pull/6) | 2026-10-03 | `e9fa408` | `fix/zoom-fit-view` | UI | test(viewer): fix the intermittent phone fit-view hang (ResizeObserver shim delivered one node per callback) |
| [#7](https://github.com/SunnyBagal/TraceHound/pull/7) | 2026-10-03 | `71cedac` | `harness/per-test-gate` | relay | Per-test regression gate (decision 043); study S1 summary |
| [#8](https://github.com/SunnyBagal/TraceHound/pull/8) | 2026-10-03 | `ee23827` | `build/to-freeze` | relay | Build to the evaluation freeze (decisions 044–046) |
| [#9](https://github.com/SunnyBagal/TraceHound/pull/9) | 2026-10-04 | `ba07667` | `build/agent-v6` | relay | agent-v6: mechanical edit fixes and re-freeze (decision 047) |
| [#10](https://github.com/SunnyBagal/TraceHound/pull/10) | 2026-10-04 | `6d2af76` | `build/reproduce` | relay | Reproduce stage for the findings pipeline (decision 048) |
| [#11](https://github.com/SunnyBagal/TraceHound/pull/11) | 2026-10-04 | `2041ec4` | `ui/wolf-logo-2` | UI | viewer: wolf head logo in silver, no backing tile |
| [#12](https://github.com/SunnyBagal/TraceHound/pull/12) | 2026-10-04 | `1b1f8fb` | `ui/canvas-polish-3` | UI | Viewer: Railway-style canvas polish (boxed canvas, bottom-left controls, Redis logo by role, tile-less node icons) |
| [#13](https://github.com/SunnyBagal/TraceHound/pull/13) | 2026-10-06 | `5a504e4` | `docs/protocol-amendment-1` | relay | docs: evaluation protocol amendment 1 and dev recount (decision 049) |
| [#14](https://github.com/SunnyBagal/TraceHound/pull/14) | 2026-10-06 | `273d842` | `docs/050-heldout-results` | relay | docs: decision 050, held-out evaluation results |
| [#15](https://github.com/SunnyBagal/TraceHound/pull/15) | 2026-10-06 | `8ed9384` | `docs/050b-followup` | relay | docs: decision 050 follow-up (evaluation prompt, drivers tarball, results page corrections) |
| [#16](https://github.com/SunnyBagal/TraceHound/pull/16) | 2026-10-06 | `075d645` | `build/051-finder-rules` | relay | Finder phase 1: graph rules that emit hypotheses, no model (decision 051) |
| [#17](https://github.com/SunnyBagal/TraceHound/pull/17) | 2026-10-06 | `5aa1070` | `build/052-finder-claims` | relay | finder phase 2a: hypotheses become reproduce claims, no model (decision 052) |
| [#18](https://github.com/SunnyBagal/TraceHound/pull/18) | 2026-10-06 | `6b31974` | `ui/railway-restyle` | UI | viewer: Railway restyle — surfaces, 2:1 cards, floating inspector, NVIDIA mark, howling logo |
| [#19](https://github.com/SunnyBagal/TraceHound/pull/19) | 2026-10-07 | `13761e6` | `docs/nebius-feedback` | relay | docs: Nebius feedback backfill + standing FEEDBACK rule |
| [#20](https://github.com/SunnyBagal/TraceHound/pull/20) | 2026-10-07 | `06de1a1` | `ui/landing-page` | UI | Viewer: landing page at /, graph viewer at /graph |
| [#21](https://github.com/SunnyBagal/TraceHound/pull/21) | 2026-10-14 | `1403747` | `docs/053-disclosure` | relay | docs: decision 053, disclosure of who saw Recall's real bugs |
| [#22](https://github.com/SunnyBagal/TraceHound/pull/22) | 2026-10-15 | `ce74480` | `build/054-bench-scorer` | relay | bench: benchmark data, scorer and judge; validation on five recorded tools (054) |
| [#23](https://github.com/SunnyBagal/TraceHound/pull/23) | 2026-10-15 | `f5b140a` | `build/055-diff-baseline` | relay | review: diff-only baseline reviewer review-diff-v1 (055) |
| [#24](https://github.com/SunnyBagal/TraceHound/pull/24) | 2026-10-15 | `f610efa` | `build/056-verify-pass` | relay | review: verification pass verify-v1 on review findings (056) |
| [#25](https://github.com/SunnyBagal/TraceHound/pull/25) | 2026-10-15 | `a38148b` | `codex/github-action` | Codex | action: GitHub Action that posts TraceHound review comments |
| [#26](https://github.com/SunnyBagal/TraceHound/pull/26) | 2026-10-15 | `5ee7677` | `build/057-context` | relay | review: repository context reviewer review-ctx-v1 (057) |
| [#27](https://github.com/SunnyBagal/TraceHound/pull/27) | 2026-10-15 | `7a92b88` | `build/058-ctx-verify` | relay | review: context-aware verifier verify-ctx-v1 (058) |
| [#28](https://github.com/SunnyBagal/TraceHound/pull/28) | 2026-10-16 | `c34b2ed` | `codex/action-fixes` | Codex | action: head recheck, example ref, Codex review rules |
| [#33](https://github.com/SunnyBagal/TraceHound/pull/33) | 2026-10-17 | `6b4276e` | `build/061-test-plan` | relay | bench: Cal.com plan + runner + one restart (061, 061a) |
| [#34](https://github.com/SunnyBagal/TraceHound/pull/34) | 2026-10-17 | `e7309a5` | `codex/fix-action-pnpm` | Codex | action: pin pnpm for external runs |

## Open or not merged

- [#35](https://github.com/SunnyBagal/TraceHound/pull/35) `ui/viewer-round-1`, head `acc5752`: viewer round 1 (no dev clutter, fresh snapshots, kind colours + legend, expandable nodes, inline code). Checked by chat 2026-10-18; OK to merge with a merge commit. Step 9 → new branch.
- [#32](https://github.com/SunnyBagal/TraceHound/pull/32) `codex/dogfood`, head `063451b`: dogfood workflow + seeded demo doc. Hold until 062 is done.
