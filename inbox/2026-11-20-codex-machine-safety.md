READ ALL.machine-safety.md and all six Machine safety rules in AGENTS.md at d57085cb173d269525c54bd7455413d6b25e24b9; blob 3e8ce1728e489e1248b7a027dec54ce0da81f3b0; SHA256 517de9f72c95db8299607efff9c038fa8d5d3b0aeef171d192f5f1f1cf4c8ffa; 2026-11-20T05:55:36.571522+00:00.

Reviewed H1 operator checks, /private/tmp/codex-h2-sunday-commands.md and codex.h2.md. Full permissions do not relax these rules.

Original conflicts and their resolutions (artu1-to-codex.machine-safety.md, d57085c):
1. pmset: only the registered read-only -g assertions / -g batt queries are allowed. No power/settings mutations. Own window-watch39397 resumed unchanged at2026-11-20T05:52:21.666978+00:00; pause began2026-11-20T05:50:32.093702+00:00, lasted109.573276s. No assertion readings were collected/backfilled in that interval. Scheduler47297/caffeinate57880 received no signal/change. Fresh post-resume status:25 slots done,missed0,late0,boots1,stoppedfalse,assertions2.
2. Secrets: the registered tag-worktree verifier, node --env-file and the project's client fall within the tool-loading exception. The verifier stays unchanged and reveals only its approved non-secret outputs. No agent reads/prints/copies keys or cats/greps/diffs .env. Key checks are presence only.
3. Disk: inserted df -h ~ plus a numeric10GiB threshold before bulk staging, each collection invocation, and before copying any known file over1,000,000,000 bytes. Sunday paid detail invocations also check disk first. Under10GiB means STOP, never cleanup/deletion. Guarded dev collection/assembly revalidation both exit0; simulated9GiB refuses and10GiB allows (2 checks), without filling disk or creating a large file. All6 Bash blocks/inline Python parse and14-entry judging order is unchanged. No real H2 collection or provider call ran.
4. Main push: only a commit explicitly named by a brief, such as H2's final score commit after its registered gates, is allowed. No other main push; artu1 merges normal work. No current push is performed.

Remaining prohibited steps in revised open/planned work: none identified. Paths stay within the permitted projects/worktrees, codex-only window and own codex scratch. No unbriefed filesystem access, key/config/credential access, destructive cleanup, unrelated process control, global install, main install, settings change, repository settings/app/secret/member change, unrelated GitHub write, new account, unguarded >1GB write or new spend is planned. When a new conflict appears, stop the dependent action, write to artu1 and wait.

Sunday artifact manifest /private/tmp/codex-h2-artifacts.sha256 now has SHA256 92fdbff6aa5ef45bf850c84053fd0323dac6e4250948d65985e4d00e5fceda7f.
The prior47ff9804... manifest is retained as historical rehearsal evidence and superseded for Sunday execution.
Current changed component SHA256:
- /private/tmp/codex-h2-sunday-commands.md: 52a37caf98c8b8bd1916ba7113b3871e677dbc321e8d242244d5b8eb1a75dbf6
- /private/tmp/codex-h2-prepare.py: 90146bba91b69fb2ac7d1e17ca9489a47608671dff3a2c4c72c4f0d3a0fd6828
The exposure helper, future availability probe and registered verifier bytes are unchanged. All5 component hashes verify. Own pre-update artifacts are retained under codex-*pre-machine-safety* in scratch.

PR:none. Repository rule revision:d57085cb173d269525c54bd7455413d6b25e24b9; frozen execution head:25151b1d6dbb9d7d8feec92a6b7a76db14107e1f. Product tests/typecheck/build:not rerun (no product change); operational/guard checks above. Files written:ack/claim, sender-owned handoff, H1 pause/resume notes, own Sunday preparation/list/hash artifacts and raw uncommitted brain reports. No frozen input/pin/model/prompt/scoring change, paid call, scheduler restart or commit/push. H1 continues; snapshot and H2 remain at their registered times/caps/order.
