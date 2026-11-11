---
id: D065
title: Evidence verifier + deterministic gate (dev only), a loss
date: 2026-10-19
kind: experiment
lane: benchmark
outcome: loss
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#065--evidence-verifier--deterministic-gate-dev-only-a-loss
pr: 48
---

# D065 · Evidence verifier + gate

## Decided
- **verify-evidence-v1:** verify-ctx-v1's context with a new system prompt. A keep must quote 1–3 exact lines and give a scenario, a severity and a category.
- **The gate (#45):** a deterministic check of that evidence.
- **Variants:**
  - A = pool-verify-v1 → gate;
  - B = pool-raw → verify-evidence-v1 → gate with evidence required and a medium severity floor;
  - C = contracts.

  The plan (`9bfb3eb`) was pushed before any model call.

## Why, at the time
verify-ctx-v1 answers keep or drop without evidence. The hope was that requiring proof, checked by code, would raise precision more than it cost recall. This was part of the v2 architecture ([P007](P007-v2-architecture.md)).

## Expected
The pre-registered conditions in [evidence-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/evidence-plan.md), against the 5-run dev means of [D064](D064-vote.md): B needed +3.0 F and +8.0 P points.

## What happened
Source: [docs/bench/evidence.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/evidence.md). The baseline mean was v1 F 42.3%.
- **A:** F 41.6%. It dropped 11 of 478 comments, 3 of them TPs.
- **B:** F 38.3% and P 37.2% (−4.0 F and −3.3 P). The verifier kept 61% of the pool, and the gate's evidence rules dropped TPs at about the pool's own TP rate.
- **C:** 0 findings on the 40 dev PRs.
- **Spend:** $2.65.

## Verdict
- **Outcome:** loss on all three variants. pool-verify-v1 stays.
- **Call:** sound. Plan first, means over five runs, and no prompt tuning after seeing scores.
- **What it rules out:** "make the verifier prove it" as a precision lever with this prompt. The gate drops real bugs as often as false ones.

## Links
[P007](P007-v2-architecture.md) · [D064](D064-vote.md) · [D066](D066-results-wording.md) (the levers table)
