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
| [#36](https://github.com/SunnyBagal/TraceHound/pull/36) | 2026-10-18 | `d37ffa8` | `build/062-test-run` | relay | bench: Cal.com blind result (062) |
| [#32](https://github.com/SunnyBagal/TraceHound/pull/32) | 2026-10-18 | `e95ef20` | `codex/dogfood` | Codex | dogfood workflow (pinned `50e1d4a`) + demo doc |
| [#39](https://github.com/SunnyBagal/TraceHound/pull/39) | 2026-10-18 | `6058d53` | `build/063-bench-results` | relay | results page + README benchmark section (063) |
| [#38](https://github.com/SunnyBagal/TraceHound/pull/38) | 2026-10-18 | `41a92a8` | `codex/fix-suggestions` | Codex | fix suggestions, `fixes: on|off`, `tracehound:v1` footer |
| [#37](https://github.com/SunnyBagal/TraceHound/pull/37) | 2026-10-18 | `df4f915` | `ui/round-1-summaries` | UI (Codex Sol) | Super summaries + names, $0.001 |
| [#35](https://github.com/SunnyBagal/TraceHound/pull/35) | 2026-10-18 | `3113c3a` | `ui/viewer-round-1` | UI (Codex Sol) | viewer round 1: no dev clutter, fresh snapshots, kinds, expand, inline code |

## Open or not merged

- [#43](https://github.com/SunnyBagal/TraceHound/pull/43) `codex/data-branch` `19ece32`: tracehound-data writer + dogfood pin + ur-01 fixes. **Merged** 19 Oct → `2af8c3a`.
- [#45](https://github.com/SunnyBagal/TraceHound/pull/45) `codex/gate-v1` `efb61d7`: S1 pre-filter + S6 gate + codex-08b fixes. **Merged** 19 Oct 13:46 (`b98f753`); full suite green on main locally.
- [#46](https://github.com/SunnyBagal/TraceHound/pull/46) `build/064-vote` `d76eeb3`: 064 vote across runs, a **loss**. **Merged** 19 Oct (`a9d16b5`).
- [#47](https://github.com/SunnyBagal/TraceHound/pull/47) `cc/contracts-v1` `d469e28`: S2b contract map + deterministic findings. **Merged** 19 Oct (`5857179`).
- [#44](https://github.com/SunnyBagal/TraceHound/pull/44) `ui/reviews-dashboard` `b9b1aee`: reviews dashboard + benchmark page. **Merged** → main `c6c12b9` (verified 19 Oct).
- [#41](https://github.com/SunnyBagal/TraceHound/pull/41) draft `2df600e`: cc-01 audit, docs only. **Merge** (chat, 19 Oct).
- [#42](https://github.com/SunnyBagal/TraceHound/pull/42) draft `433212c`: ReviewBench spike. Leave it as a draft until the ReviewBench decision; close it if that's a no.

- [#50](https://github.com/SunnyBagal/TraceHound/pull/50) draft `rb/image-v1` `1d06203`: ReviewBench image (rb-01). rb-01b landed (`6bb935c`, 38 tests). **Cleared to merge** (chat, 20 Oct 19:15). After merge: rebuild on main, then 3 smoke runs.
- [#49](https://github.com/SunnyBagal/TraceHound/pull/49) `30a3d44`: codex-09 learning. codex-09b landed (`9b21e70`). **Cleared to merge** (chat, 20 Oct 19:50).
- [#51](https://github.com/SunnyBagal/TraceHound/pull/51) draft `rb/missed-v1` `99f35d0`: rb-02, review-missed-v1 + pool-raw-missed (P008 candidate 6). **Cleared to merge** (chat, 20 Oct 19:50). Prompt sha256 recorded in P008.
- [#52](https://github.com/SunnyBagal/TraceHound/pull/52) draft `docs/066-results` `0d3a6c4` (Bunny): 066 results wording, all-50 tables, levers table, viewer sync. Checked by chat 20 Oct 20:50; 066c → `e91a268`. **Cleared to merge** (21:00).

- [#52](https://github.com/SunnyBagal/TraceHound/pull/52) **merged** → main `2a95756` (verified 20 Oct 21:20).
- [#53](https://github.com/SunnyBagal/TraceHound/pull/53) draft `codex/int-01-contracts` `747a3a7` (Codex): contracts in the Action, default on, fail-soft. Checked by chat 20 Oct 21:40: no HOLD; merge after Ultrareview #2. Backlog in NOW.
- [#54](https://github.com/SunnyBagal/TraceHound/pull/54) draft `docs/judges-readme` `db83286` (Bunny): judges' README with placeholders. Checked by chat 20 Oct 21:40: **leave it a draft** until the placeholders are filled (26 Oct).
- [#53] **HOLD** (chat, 21 Oct 00:10): Ultrareview #2 found 7 confirmed issues → [codex-11](prompts/2026-10-21/codex-11-int-01-fixes.md) on the same branch.
- [#53] codex-11 → head `3b196fa` (7 fix commits). Checked by chat 21 Oct 17:10: **cleared to merge**. Residual (analyzer budget covers only typed) goes to the backlog.
- [#53] **merged** → main `7de5dbd` (verified 21 Oct 20:30). Freeze-2 re-pin target.
- [#55](https://github.com/SunnyBagal/TraceHound/pull/55) draft `fix/model-base-url` `5ec764d` (Bunny, bunny-03): per-model base URL (3.5 → us-north1). Checked by chat 21 Oct 21:00: **cleared to merge once CI is green**.
- [#55] **merged** → main `da13585` (21 Oct).
- [#56](https://github.com/SunnyBagal/TraceHound/pull/56) draft `build/067-dev-3.5` `46a678d` (relay 067b): 3.5 adopted on dev (2 runs). Checked by chat 22 Oct 04:15: **cleared to merge**. Default switch + partial results → codex-12 (068).
- [#56] **merged** → main `8d0fb7a` (verified 23 Oct).
- [#57](https://github.com/SunnyBagal/TraceHound/pull/57) draft `codex/068-default-3.5` `4e3a0a8` (Codex, codex-12, decision 068): 3.5 default + partial reviews. Checked by chat 23 Oct 16:40: **cleared to merge**.
- [#57] **merged** 23 Oct 16:25 IST → main `0aeef06` (merge commit; CI green; verified 17:15).
- [#58](https://github.com/SunnyBagal/TraceHound/pull/58) draft `docs/devpost-drafts` `3d5a62d` (Bunny, bunny-04): Devpost drafts in `docs/devpost/`. Checked by chat 23 Oct 20:05: fine, **stays a draft** until the 26–27 Oct fill; hold the "no script from the PR" sentence until codex-13b Q9.
- [#59](https://github.com/SunnyBagal/TraceHound/pull/59) draft `build/069-v2-calcom` `7f757c1` (relay, 069): v2 blind Cal.com run. Checked by chat 23 Oct 20:05: **cleared to merge** (merge commit).
- [#59] **merged** 23 Oct → main `5fb8cdc` (verified 20:55).
- [#60](https://github.com/SunnyBagal/TraceHound/pull/60) draft `build/069b-v2-intervals` `aec07e3` (relay, 069b): intervals. Checked by chat 23 Oct 22:10: **cleared to merge**.
- [#61](https://github.com/SunnyBagal/TraceHound/pull/61) draft `docs/contracts-boundary` `eef0604` (Bunny, bunny-05): contracts security boundary docs. Checked by chat 23 Oct 22:10: **cleared to merge**.
- [#54] bunny-05 → head `1c57c72` (still draft). [#58] → `ff483fc` (still draft).
