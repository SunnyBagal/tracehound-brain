---
id: D069
title: The v2 blind Cal.com run (second look at the test set), with 069b intervals
date: 2026-10-23
kind: experiment
lane: benchmark
outcome: tie
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#069--the-v2-blind-calcom-run-second-look-at-the-test-set
pr: 59 (069b: 60)
---

# D069 · v2 blind Cal.com run

## Decided
Freeze-2 (`review-freeze-2` → `0aeef06`) changed one thing for the benchmark: every pool-verify-v1 stage now runs on 3.5 Super VL. The plan was:
- one Cal.com run of v2, stated as the second look, as P007 allows;
- runner gated to `TRACEHOUND_TEST_RUN=069`, with a $3 cap;
- pre-registered in `docs/bench/v2-calcom-plan.md` (`b2ec214`, pushed 48 s before the first Cal.com call).

069b added PR-level paired bootstrap intervals (plan pushed before the first resample).

## Why, at the time
The model swap ([D067](D067-nemotron-35.md)) had been chosen on dev. Only the test set could check it, and the architecture plan allowed exactly one more look.

## Expected
- (a) F ≥ 52.8%;
- (b) P ≥ 55.6%;
- (c) at most 3 of 10 PRs with a failed chunk.

In 069b: "ahead" or "behind" only when a 95% interval excludes 0.

## What happened
Sources: [docs/bench/v2-calcom.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/v2-calcom.md), [v2-intervals.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/v2-intervals.md).
- **The run:** n = 23, one run, $0.72. v1 P 63.2% / R 52.2% / F 57.1%, and x1 F 58.5%. All three predictions were right, and 1 of 10 PRs had a failed chunk.
- **Cal.com, 10 PRs:** statistically tied with Augment, Cursor, Greptile, Copilot and 3 Super's run 1, and ahead of CodeRabbit.
- **All 50 PRs:**
  - behind Augment (v1 −14.0 [−22.6, −4.7]);
  - tied with Cursor and Greptile;
  - ahead of Copilot and CodeRabbit.

## Verdict
- **Outcome:** tie. It matches the 3 Super result within its spread, and the model swap shows no interval-backed change on the test set.
- **Call:** sound. One look, pre-registered with a timestamp, and the claims were set by intervals afterwards.
- **What it rules out:** Cal.com as an evaluation set. It is spent ("Cal.com is spent", REPORT §1), so later held-out claims need a fresh set ([Track B](../benchmark/track-b-testset.md)).

## Links
[D067](D067-nemotron-35.md) · [D068](D068-partial-reviews.md) · [P007](P007-v2-architecture.md) · [results](../results.md)
