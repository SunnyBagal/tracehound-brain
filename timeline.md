# Timeline

From git history on TraceHound main (merge dates) and decisions made in chat. PR detail: [prs](prs.md). Decision detail: [digest](decisions/digest.md).

## Phase 1: graph ([history](history/phase-1-graph.md))

- **2026-09-30** First commit in TraceHound's history: pnpm workspace, CLAUDE.md, the decision log. Decisions 001–031 appear the same day, likely carried over from earlier work. Viewer canvas polish (#1). Repair agent v2 fails the toy gate at 2/5.
- **2026-10-01** Analyzer 0.7.0 with BullMQ (035). Docker becomes the sandbox of record after Nebius Sandboxes turn out to be unavailable (036). Naming audit finds most summaries wrong. Canvas polish 2 (#2).
- **2026-10-02** Change sets (038), multi-repo index and version guard (039); Recall published as the second repo.
- **2026-10-03** Change view (#3). Repair harness on Recall (#4, 041). BullMQ through DI, analyzer 0.10.0 (#5, 042). Per-test gate and study S1: 2.1% pairing on unseen repos (#7, 043). Build to the evaluation freeze (#8, 044–046).

## Phase 2: repair evaluation ([history](history/phase-2-repair-eval.md))

- **2026-10-04** agent-v6 and re-freeze (#9, 047). Reproduce stage (#10, 048). Wolf logo, canvas polish 3 (#11, #12).
- **2026-10-05/06** Held-out evaluation runs: 160 counted runs, $5.72.
- **2026-10-06** Protocol amendment 1 (#13, 049). **Result: the graph did not help** (#14, #15, [D050](decisions/D050-graph-did-not-help.md)).

## Phase 3: finder ([history](history/phase-3-finder.md))

- **2026-10-06** Finder rules (#16, 051) and finder claims (#17, 052). Railway restyle (#18).
- **2026-10-07** Nebius feedback backfill (#19). Landing page (#20). **Pivot: TraceHound becomes a PR reviewer** ([P001](decisions/P001-pivot-to-reviewer.md)).

## Phase 4: reviewer ([history](history/phase-4-reviewer.md))

- **2026-10-14** Disclosure of who saw Recall's bugs (#21, [D053](decisions/D053-recall-disclosure.md)). Codex and Grok lanes set up ([P002](decisions/P002-lanes-and-tools.md)). Relay per-run Nebius cap set to $3.
- **2026-10-15** Scorer and judge (#22, [D054](decisions/D054-review-benchmark-scorer.md)). Diff-only baseline, F 32.2% (#23, [D055](decisions/D055-diff-only-baseline.md)). Verifier v1 loss (#24, [D056](decisions/D056-verify-v1.md)). Grok's first audit passes the kill test ([P003](decisions/P003-grok-kill-test.md)). GitHub Action (#25). Repo context ties, F 31.8% (#26, [D057](decisions/D057-repo-context.md)). Context verifier F 32.8%, below the 35.0% bar, but it separates TPs from FPs (#27, [D058](decisions/D058-verify-ctx.md)). This vault started.
- **2026-10-16** Action fixes, Codex review rules, lockfile fix (#28). Sunny picks pooling before scorer fairness. 059 pool-verify-v1: **F 35.6%, first variant over a pre-set bar, adopted**. The raw pool reaches 51.4% recall (PR #29, [D059](decisions/D059-pool-verify.md)). Codex switches the Action to pool-verify-v1 (#30). 060: a Qwen judge confirms the ranking (pool-verify-v1 5th of 11 under both judges; F 44.6 under Qwen); the validity check is too lenient to use (#31, [D060](decisions/D060-scorer-checks.md)). Grok research R1–R3 filed, all citations real.

## Ahead

23 Oct freeze and the Cal.com run → 24–25 Oct results doc → 26 Oct Devpost write-up → 29 Oct submit. See [roadmap](project/roadmap.md).
