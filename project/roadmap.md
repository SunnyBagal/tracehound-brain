# Roadmap: planned, built, cut

## The reviewer plan of 7 Oct, against what exists

`docs/reviewer-plan.md` (7 Oct) planned this pipeline. This shows what happened to each step.

| Planned step | Status |
|---|---|
| Deterministic context: declaration-level diff, callers of changed declarations, related tests, finder rules on the diff | **Changed.** Built as language-agnostic `git grep` context (057, 058). The TS-only change sets and finder rules were not used, because 4 of 5 benchmark repos aren't TypeScript |
| One context packet per changed declaration | Changed: one block per diff chunk (057), then one per finding (058) |
| Candidate findings, one model call per packet | Built as one call per diff chunk (review-diff-v1, 055) |
| Refuter pass that drops weak candidates | Built twice. verify-v1 (056) was a loss. verify-ctx-v1 (058) separates TPs from FPs but didn't reach the bar. 059 tests it on a wider pool |
| Reproduce stage for a "proven by a test" tier | **Cut** from the benchmark run. Recall-only demo only if 26 Oct has time |
| Line comments in tiers plus a per-PR graph view with findings pinned to nodes | Line comments via the Action: built. Tiers and graph view: not built |

Also cut on 7 Oct: more repair-agent work, Nebius Sandboxes (Contree), the three-bug comparison on Recall as a headline, and finder phase 2b.

## Schedule from 16 Oct

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 pool-verify-v1: **adopted** (F 35.6) | — (#28 merged) |
| 17–18 Oct | 060 scorer checks: second judge (Qwen3-235B) and augmented precision | Action switched to pool-verify-v1 (codex-03), then the **demo repo** |
| 19–20 Oct | Spare, or a second precision round | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the **one** Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md cleanup, "what changed since 26 Aug" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct | |

## Fallbacks agreed in advance

- If context doesn't beat diff-only: ship the better one and report that context didn't help. **Triggered by 057.**
- If the verifier lowers dev F: ship without it and say so. **Triggered by 056; 058 missed the bar.**
- If no variant clears F 35%: freeze review-diff-v1 and run it on Cal.com as is.

## After the hackathon (graph roadmap from CLAUDE.md, parked)

API (Fastify) + Postgres (Drizzle) + BullMQ jobs; more extractors (Kafka, fetch/axios between services, tRPC, Next routes); snapshot diffing across commits. The graph-guided repair agent is parked after [D050](../decisions/D050-graph-did-not-help.md).
