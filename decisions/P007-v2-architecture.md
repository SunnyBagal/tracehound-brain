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

## After 064 and 065 (chat, 19 Oct 16:30; Sunny to confirm)
- Three precision levers lost on dev in a row: verify-ctx (058), vote (064), evidence + gate (065). The adopted reviewer's own run-to-run spread (12 F points) is bigger than any effect tried.
- **Proposed scope cut:** stop precision tuning. The benchmarked reviewer stays pool-verify-v1, unchanged. v2 = pool-verify-v1 + **deterministic contract findings** (measured on the seeded test) + **learning** (demo only) + actionable delivery. The gate and verify-evidence stay merged as libraries; neither is wired in, because neither was adopted.
- **The top 2 across all 50 is not reachable by 29 Oct** on current evidence. The pitch becomes an honest benchmark, cross-file breaks on a pre-registered seeded test, and a product that learns, all on an open model at cents per PR.
- The one untried lever with a real reason behind it is temperature 1.0 / top_p 0.95 (NVIDIA's recommendation; we run 0). A 5-run dev test costs about $9 of the remaining $19. Chat's recommendation: skip it.

## Links
- Prompts: `prompts/2026-10-19/codex-08-gate.md`, `cc-03-contracts.md`, `grok-07-competitors-learning.md`.
