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
| 16 Oct (late) | 060 done: judges agree; validity check too lenient | #30 Action on pool-verify-v1 |
| 17 Oct | 061 test plan: pre-registration and run code, no model calls | codex-04: dogfood on TraceHound + seeded-bug demo repo |
| 18–19 Oct | 062: tag `review-freeze-1`, then the one Cal.com run | Demo results |
| 20 Oct | Results doc | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Spare (the freeze moved to 18–19 Oct) | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md cleanup, "what changed since 26 Aug" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct | |

## Fallbacks agreed in advance

- If context doesn't beat diff-only: ship the better one and report that context didn't help. **Triggered by 057.**
- If the verifier lowers dev F: ship without it and say so. **Triggered by 056; 058 missed the bar.**
- If no variant clears F 35%: freeze review-diff-v1 and run it on Cal.com as is.

## Product backlog (Sunny, 2026-10-17), in priority order

| # | Feature | Where | When |
|---|---|---|---|
| 1 | **Kind colours + legend; one-line node summaries (Super, facts only, Sunny checks)** | UI lane, ui-01 addendum | now |
| 2 | **Persistent graph per commit and per PR.** The Action also runs the analyzer: on push to main it writes a snapshot; on each PR it writes a change set (`tracehound changes`, base..head). Both go to an orphan branch `tracehound-data` in the target repo. The viewer reads them from raw.githubusercontent.com, giving history, a PR overlay, and "merged → moves to history". No server. TS repos only (analyzer limit) | Codex (codex-07, after 06) + UI (ui-02) | 20–22 Oct |
| 3 | **File-level view:** expand a component into its most-used files as child nodes (ranked by importers and routes), the rest in a dropdown. Viewer-side, from snapshot file facts; no analyzer change needed | UI lane | ui-02/03 |
| 4 | **Flow trace** ("login → … → final execution"): pick a route; the route pops out of its parent node; follow resolved declaration-level calls (change-set call edges, decision 038) node to node, each step lit, with evidence code | UI + analyzer (after the Cal.com run unfreezes analyzer src) | if time 24–26 Oct, else after |
| 5 | **Other providers** (Claude, OpenAI/Codex, Grok) behind one provider interface. Token Factory and local servers (Ollama, LM Studio, vLLM) are OpenAI-compatible; Anthropic needs an adapter. **Bring your own model + see its benchmark score** reuses the scorer | analyzer `llm/` | after 30 Oct (hackathon rule: Nemotron is the engine) |
| 6 | **Local models** to review PRs and explain a codebase offline | same interface, local endpoint | after 30 Oct |
| 7 | Review memory: learn which findings a repo dismisses | Action + data branch | after 30 Oct |
| 8 | Search / command palette, minimap, evidence on edge hover | UI | when time allows |

## After the hackathon (graph roadmap from CLAUDE.md, parked)

API (Fastify) + Postgres (Drizzle) + BullMQ jobs; more extractors (Kafka, fetch/axios between services, tRPC, Next routes); snapshot diffing across commits. The graph-guided repair agent is parked after [D050](../decisions/D050-graph-did-not-help.md).
