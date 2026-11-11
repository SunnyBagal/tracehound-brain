---
id: D070
title: recall-v3 for ReviewBench, built and dev-selected; neither arm qualifies
date: 2026-10-24
kind: experiment
lane: benchmark
outcome: loss
call: flawed
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#070--recall-v3-for-reviewbench-built-dev-selected-neither-arm-qualifies-2026-10-24
pr: 62
---

# D070 · recall-v3

## Decided
Build `recall-v3` ([P009](P009-reviewbench-v3-recall.md)) as an additive pipeline (`tracehound review-recall`). The other pipelines stay byte-identical. Run it once on dev per arm (S = 3 Super, V = 3.5 Super VL) and apply P009's selection.

## Why, at the time
ReviewBench's grounded metric rewards recall ([R16](../research/R16-reviewbench-grounded-f1.md)). P009 proposed a recall mode for the final.

## Expected
P009's floors are **precision** floors (v1 precision, the same scorer as the other dev results): arm S needs P ≥ 20.0, and arm V needs P ≥ 38.0 ([v3-recall.md:9-12](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/v3-recall.md)).

## What happened
Source: [docs/bench/v3-recall.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/v3-recall.md).
- **Arm S:** P 14.0 / R 50.0 / F 21.8. **P 14.0 < 20.0: fails its precision floor.**
- **Arm V:** P 20.9 / R 63.5 / F 31.4. **P 20.9 < 38.0: fails its precision floor.**
- **Recall:** V raised recall over 3.5's pool-raw dev mean (63.5 vs 61.5); S did not (50.0 vs 52.7). Both paid with precision far below the floor.
- **Reasoning budget:** Token Factory honours no reasoning token budget for either model (`reasoning_budget` is ignored), so both arms used a fallback.
- **The final would have cost** S $36.29 / V $54.70.
- **Disclosed reading:** the stop rule's "budget refusal" was read as the client's global/run cap, not recall-v3's own per-PR cap. That cap refused 15 times in arm S, and the run went on to arm V.

## Verdict
- **Outcome:** loss. Both arms fail their precision floors, so v3 stops, and the P008 winner gets the final.
- **Call:** flawed, as recorded in [lessons](../lessons.md) (2026-10-24). Rechecked after the precision-floor correction (2026-11-11), the call stands: it rests on the process, not on the floor metric.
  - the stop rule didn't name every refusal source;
  - P009's per-PR cap was never checked against reservation-based budgeting, so v3 was measured partly crippled.
- **What we'd do differently:** list every refusal source by name in a stop rule. Check any per-PR cap against the client's worst-case reservation before it goes into a pre-registration.
- **What it rules out:** a reasoning-token budget as a cost or length control on Token Factory (confirmed again in [D074](D074-hard-chunks.md)).

## Links
[P008](P008-reviewbench-preregistration.md) · [P009](P009-reviewbench-v3-recall.md) · [R16](../research/R16-reviewbench-grounded-f1.md)
