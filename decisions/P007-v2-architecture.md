---
id: P007
title: Reviewer v2 architecture — gate hard, add cross-file contracts, learn from feedback
date: 2026-10-19
kind: product
lane: chat
outcome: pending
call: too-early
judged_by: 
judged_on: 
repo_entry: none
pr: 
---

# P007 · Reviewer v2 architecture

## Decided (proposed by chat 19 Oct 01:30; Sunny to approve)
- Pipeline S1–S8 as in [architecture-v2](../project/architecture-v2.md): pre-filter → contract map → generators → vote → evidence verifier → deterministic post-gate → deliver → learn.
- Sunny's four focus areas, mapped onto the stages: noise (S1, S4, S5, S6), cross-file (S2b, S3 contracts), actionability (S5, S7), learning (S8).
- No webhook server or App, no generic specialist swarm, no IDE plugin before the freeze, no embeddings for S8.
- 065 is not run alone. After freeze-2 (24 Oct) there is one blind Cal.com run of v2, stated as the second look at that set.
- Ultrareview #2 goes to the int-01 integration branch instead of 064; #3 to the final pre-submission branch.
- Linea and Recall are a demo and a pre-registered seeded cross-file test, not a benchmark comparable with the tools.

## Why
- All 50 PRs: TraceHound has 57 FPs; Augment has 16 and Cursor 17. Our recall of 41.2 is close to Cursor's 43.3. Precision decides the top 2.
- D057 and D058: more context made the model more conservative, not sharper. Context helps when it judges, not when it generates.
- D050: the model never used the graph as a tool. So the graph feeds deterministic contract findings instead.
- R4: the precision of the top tools comes from post-generation gating and verification, not from more generators.

## Expected (written before any result)
- Top 2 on all 50 PRs needs F above 53.8 (Cursor). At R ≈ 45, that means P ≈ 65. Plausible, not promised. Beating Augment (66.3) is not expected.
- The S8 learning stage moves no benchmark number; it is shown as a scripted demo only.

## Links
- Prompts: `prompts/2026-10-19/codex-08-gate.md`, `cc-03-contracts.md`, `grok-07-competitors-learning.md`.
