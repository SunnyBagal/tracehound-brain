---
id: D061
title: Pre-registered plan for the one Cal.com run, and the code it needs
date: 2026-10-16
kind: process
lane: benchmark
outcome: pending
call: too-early
judged_by:
judged_on:
repo_entry: pending
pr:
---

# D061 · Test-set plan (freeze)

## Decided (Sunny, 2026-10-16)
- Stop tuning after 060; don't run a fifth verifier variant. The reason is selection bias, plus time better spent on the demo and video (judges may score from the video alone, per R3).
- 061 writes the plan and the run code with no model calls and no Cal.com data. 062 tags `review-freeze-1` and executes the plan.

## The plan (to be written into the repo by 061)
- **Primary:** pool-verify-v1.
- **Declared secondaries:** review-diff-v1 and pool-raw, from the same pipeline. Plus the five tools.
- **Scorer:** bench-judge-v1 is the scorer of record; bench-judge-x1 is reported alongside. No validity check.
- **Runs:** one run. One rerun is allowed only for an infrastructure failure, and the original is kept.
- **Predictions** are written before the run and scored afterwards. Cap $5.

## Verdict
Pending (after 062).
