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
- **Runs:** Cal.com run 1 is the pre-registered headline. Runs 2–3 on Cal.com and runs 2–3 on dev measure run-to-run spread, with response-cache reads off. They were added 2026-10-17 because the balance is $46. One rerun is allowed only for an infrastructure failure, and the original is kept.
- **Predictions** are written before the run and scored afterwards. Cap $15 (run 1 is never cut).

## Verdict
Pending (after 062).
