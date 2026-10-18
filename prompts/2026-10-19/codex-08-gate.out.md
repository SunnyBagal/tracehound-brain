# codex-08 output (Codex, pasted by Sunny 19 Oct 03:23). Checked by chat: see NOW / prs.

Draft PR #45 (https://github.com/SunnyBagal/TraceHound/pull/45) targets main. CI passed; no fix attempt was needed. Worktree is clean.

Head: b19f5eb8c481a93456c480cffd53263249ee4282

- 1a81bb4 — Diff pre-filter, config field, fixtures and tests.
- 56f7b2d — Ordered findings gate, reviewed nit patterns and tests.
- b19f5eb — Offline CLI, compatibility tests and documentation.

Local validation: 903 passed, 7 skipped, including 68 gate tests. Typecheck passed in all three workspace projects.

Changed: four src/gate/*.ts modules, bin.ts, config.ts, test/gate.test.ts, three gate fixtures, and docs/gate.md.

Deferred: suppression persistence and Action wiring. Relay-held outputs were not accessed; compatibility tests use synthetic fixtures. pool-verify-v1 remains unchanged.

| Rule id | Positive / negative test cases |
|---|---|
| lockfile | All listed filenames / similarly named source |
| generated | Paths, suffixes, early markers / handwritten source, late markers |
| snapshot | Snapshot directory and suffix / ordinary source |
| binary | Both Git binary headers / text diff |
| whitespace | Equivalent hunk removed / substantive hunk retained |
| config-ignore | Matching globs / unmatched or unconfigured paths |
| prefiltered | Removed path / retained or partially filtered file |
| not-in-diff | Invalid locations / added, context and valid exempt paths |
| evidence-missing | Required absent evidence / present evidence or option unset |
| evidence-mismatch | Missing or mismatched quote / trimmed multiline quotes within ±3 |
| style | Category and every nit fixture / functional counterexamples |
| suppressed | Stub fingerprint match / unmatched or empty suppressions |
| duplicate | Normalized paths within 3; ranking ties / distinct paths, distant or null lines |
| severity-floor | Below floor / equal, higher, unspecified or unset floor |
| cap | Ranked overflow and zero cap / within limit or unset cap |

Idempotence is covered across 80 generated pools and overlapping duplicate neighborhoods.
