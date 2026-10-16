# Phase 1: the evidence-backed graph (decisions 001–044, ≤ 3 Oct)

**Goal:** turn a TypeScript repo into a component graph where every edge points to code: file, symbol, line range, extractor, confidence. No model draws an edge.

**Note on dates:** decisions 001–031 first appear in git on **30 Sep 2026**, the day TraceHound's history begins ("docs: CLAUDE.md and decisions log"). They were most likely carried over from earlier work (FixFlow). This matters for the hackathon's "what changed since 26 Aug" summary; see [hackathon](../project/hackathon.md).

## What was built

| Area | What | Decisions |
|---|---|---|
| Runtime | Node 24 native type stripping, TS 6 for typecheck | 001, 002 |
| Grouping | Anchor reachability (entry points, route modules, infra wrappers claim the files only they reach); each file records why | 003, 008, 010 |
| Edges | Redis per connection; queue edges follow data flow; values resolved with decaying confidence (1.0 / 0.9 / 0.7 / 0.5); labels `proven` · `resolved-default` · `dynamic` | 004, 005, 007, 011, 017 |
| Snapshots | Static JSON per (commit, analyzer version) with embedded snippets, plus a manifest; no API | 006, 009, 018 |
| Model use | Optional naming pass on Nano with reasoning off, facts only, fail-safe, with deterministic name checks; hard budget caps; response cache | 012, 013, 014, 020, 021 |
| Viewer | ELK layered left-to-right, floating edges, fact-backed tech icons, capped zoom, Railway-style restyle; on Vercel | 015, 016, 019, 028, 031 |
| Impact and agent tools | Impact analysis direction rules; a queue costs one hop; lexical context packets; MCP tools | 023, 024, 025, 027 |
| BullMQ | Detector `bullmq-queues@0.1`, then `@0.2` (through one dependency-injection hop); analyzer 0.10.0 | 035, 042 |
| Change sets | Declaration-level diff with an architectural rollup, change view in the viewer, multi-repo index, version guard | 038, 039, 040 |
| Hygiene | Every git spawn drops `GIT_*` variables (after a hook once wrote fixture commits into a real worktree) | 032 |

## Results and verdicts

- **Works on its home repos.** CEX @ `da0e3d6`: 9 components, 17 edges. Recall @ `5d2165a`: 7 components, 6 edges.
- **Naming quality was poor.** Audited by hand on 1 Oct: Recall had 5 of 7 summaries wrong, CEX 2 of 7. Fixes went in as config overrides, never hand-written summaries. Nano beat Super on cost and tied on the checks.
- **Didn't transfer to unseen repos.** Study S1 (pre-registered, ten unseen repos): the BullMQ pairing rate was **2.1%** (4 of 179 producers). Both kill criteria were met, so "queue contract checker as a product" was dropped. Most real code reaches queues through getters, wrappers, enums or runtime names.
- vs dependency-cruiser on 3 seeded branches: it found nothing TraceHound missed, but TraceHound over-reports at component granularity (`docs/impact-comparison.md`).

**Verdict:** the graph is solid engineering, and its evidence-first rule is a real differentiator. But it only works on repos it was shaped on, and S1 is the honest proof. Sound calls throughout. The mistake, in hindsight, was assuming the graph would be the product's engine before testing it on unseen code.
