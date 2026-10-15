# Timeline

Dated from git history on TraceHound main (merge dates), plus decisions made in chat. One line per event; link out for detail.

## Before the pivot: graph and repair agent

- 2026-09-30 First commit in TraceHound's history: pnpm workspace, CLAUDE.md and the decision log.
- 2026-10-01 Analyzer 0.7.0 (BullMQ queues); Docker becomes the sandbox of record (decision 036).
- 2026-10-02 Change sets (decision 038), the multi-repo snapshot index (039), Recall published as the second repo.
- 2026-10-03 Recall on the repair harness (041); per-test gate; the eval freeze (`eval-freeze`).
- 2026-10-04 agent-v6 (047) and the reproduce stage (048).
- 2026-10-06 Protocol amendment 1 (049); held-out results: **the graph did not help the repair agent** ([D050](decisions/D050-graph-did-not-help.md)). Finder phases 1 and 2a (051, 052).
- 2026-10-07 Landing page and Nebius feedback merged. **Pivot: TraceHound becomes a PR reviewer** measured on a public benchmark ([P001](decisions/P001-pivot-to-reviewer.md)).

## After the pivot: the reviewer

- 2026-10-14 Disclosure of who saw Recall's bugs merged ([D053](decisions/D053-recall-disclosure.md)). Codex and Grok lanes set up ([P002](decisions/P002-lanes-and-tools.md)).
- 2026-10-15 Benchmark scorer ([D054](decisions/D054-review-benchmark-scorer.md)), diff-only baseline ([D055](decisions/D055-diff-only-baseline.md)), verifier loss ([D056](decisions/D056-verify-v1.md)) merged.
- 2026-10-15 Grok's first audit passes the kill test ([P003](decisions/P003-grok-kill-test.md)).
- 2026-10-15 GitHub Action merged (#25, `a38148b`). Repository context ties ([D057](decisions/D057-repo-context.md), #26, `5ee7677`).
- 2026-10-15 058 context-aware verifier: F 32.8%, below the pre-set 35.0% bar; it separates TPs from FPs (keeps 69% of TPs, 33% of FPs) but loses recall ([D058](decisions/D058-verify-ctx.md), PR #27).
- 2026-10-15 Action fixes and Codex review rules (PR #28). This vault started.
