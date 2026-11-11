---
id: D074
title: Pre-registered recovery of empty replies on the hard-chunk set
date: 2026-11-08
kind: experiment
lane: benchmark
outcome: win
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#074--pre-registered-recovering-empty-replies-on-the-hard-chunk-set-2026-11-08
pr: none (branch bench/hard-chunks-074, merged as 66e3d8a)
---

# D074 · Hard chunks: recovering empty replies

## Decided
**The set:** the 14 distinct reviewer chunk requests behind the 20 failures in 3.5 dev runs 7r/8/9. Each was drawn fresh 3 times, giving 42 paired units, with the cache bypassed (the new standing rule).

**The arms**, after an empty first reply:
- **A:** the current append retry;
- **D:** split the chunk in half;
- **E:** one retry with reasoning off and a JSON-schema `response_format`.
- **C dropped:** a reasoning-token budget is undocumented on Token Factory, so arm C was dropped before any call.

**Cap:** $4. Codex gave GO after three reviews.

## Why, at the time
073a showed that most lost chunks began with an **empty** first reply, which no parser can recover ([empty replies](../project/empty-replies.md)).

## Expected
An arm wins if all of these hold:
- its successes ≥ max(1.5 × A's, A's + 1);
- its cost per unit ≤ 1.5 × A's;
- at least 30 complete paired units.

Nothing is adopted from the result: finding quality is to be checked first.

## What happened
Source: [docs/bench/hard-chunks.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/hard-chunks.md).
- **Coverage:** 42 of 42 units, $3.36463.
- **First replies:** 26 were empty, all ending `length` with reasoning using all 32,000 output tokens.
- **Successes:**

  | arm | all 42 units | empty-first units (26) | cost per unit |
  |---|---|---|---|
  | A | 22 | 7 | $0.0472 |
  | D | 30 | 15 | |
  | E | 41 | 26 | $0.0329 |

- **By the gate:** E wins, and D does not.
- **What E's replies looked like:** all ended `stop` with 0 reasoning tokens. The model has no "JSON mode" tag, so whether the schema was enforced is unknown.
- **Claim scope (devin's wording):** it shows only that E returns anchored findings after an empty reply more often. It does not show that those findings are correct.

## Verdict
- **Outcome:** win by its gate, but superseded. [D075](D075-verify-a-vs-e.md) then found that E's extra findings did not become kept true positives, and E was dropped on 2026-11-09.
- **Call:** sound. It used fresh draws as the new rule requires, dropped the infeasible arm before spending, and kept the claim limited.
- **What it rules out:** splitting chunks (arm D) as the fix for empty replies.

## Links
[D073](D073-recovery-ab.md) · [D075](D075-verify-a-vs-e.md) · [empty replies](../project/empty-replies.md) · [ledger vs billing](../ops/ledger-vs-billing.md) (074's charge and the console lag)
