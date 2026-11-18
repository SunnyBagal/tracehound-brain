# fable · side track C: exposure count, two pre-registrations, lookup fix · 2026-11-16

Raw report. Dev only; counts and method only. Not reviewed (needs codex's GO before it is committed or pushed).

## Done
- Branch `claude/upbeat-lamport-b280zm`, `ccf6c01..6e51a21`, pushed. New paths only; nothing on `main`, in the frozen
  reviewer or on the forks was touched. No model call, $0.
- **Training exposure of the 40 dev cases** (from the dev export's fix shas and the dev clones, read-only git, no model):
  of 31 bug cases, 0 fix commits are dated before 1 July 2026 and 31 after (the cutoff classifications agree for 31 of 31 fixes; the exact calendar dates agree for 30 of 31); the
  earliest is 1 day after, the latest 126 days after. The bug-introducing commit is dated before 1 July 2026 in 24 of 31.
  Published training-data cutoff of Claude Sonnet 5.5 and Opus 5.5: June 2026, to the month. Not checked: bug reports or
  discussion before the fixes.
- **`f827b23` pre-registrations, committed before any run:** `sidetrack/ab2-plan.md` (Sonnet + skill with no
  affected-set packet against the short packet) and `sidetrack/ab3-plan.md` (Sonnet pooled with Opus 5.5, merged and
  de-duplicated, against Sonnet alone). Both: two draws per arm, bench-judge-v1 as scorer of record, under 4 catches of
  31 is no detectable change, a keep needs no more comments on the 9 clean cases. Both record the author's prediction.
- **`082b47d` lookup `affected@0.5`:** a declaration the diff only removes lines from is now in the affected set.
- **`601dafe` runner `track-c-runner@0.3`:** `--packet none`; the packet arms' prompt is byte-identical to `ccf6c01`.
- **`6e51a21` `scripts/track-c/pool-reviews.ts`:** A/B 3's merge rule in code (decision 058's agreement rule, cap 6).
- Gates in the branch's worktree: typecheck clean; analyzer 1,083 passed, 8 skipped (13 new tests).

## Not done
- No arm was run or scored. bunny runs and scores them later; judging needs artu1's approval and a cap.
- `.agents/STATE.md` and `docs/decisions.md` were not edited (artu1's files): neither plan has a decision entry yet.
