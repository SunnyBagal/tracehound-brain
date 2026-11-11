---
id: D062
title: Result of the pre-registered Cal.com test-set run (061 as amended by 061a)
date: 2026-10-18
kind: experiment
lane: benchmark
outcome: tie
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#062--result-the-calcom-test-set-run-decision-061-as-amended-by-061a
pr: 36
---

# D062 · The Cal.com test-set run (first look)

## Decided
Run 061's plan exactly as written, from the tag `review-freeze-1` (on `6b4276e`). Cal.com run 1 is the headline. Cal.com runs 2–3 and dev runs 2–3 measure spread only. The cap was $15.

## Why, at the time
Tuning had stopped after 060. pool-verify-v1 (059) was the configuration under test, and Cal.com was the only part of the dataset never used for selection ([D061](D061-test-plan.md)).

## Expected
Three predictions, written in 061 before the run:
- (a) pool-verify-v1's Cal.com F would be above review-diff-v1's;
- (b) it would rank 5th of six;
- (c) its Cal.com F would be more than 5 points away from its dev F (outside 30.6–40.6%).

## What happened
Source: [docs/bench/test-results.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/test-results.md). Cal.com run 1, n = 23, bench-judge-v1:
- **pool-verify-v1 had F 58.3%; review-diff-v1 had 53.8%.** It ranked 3rd of six as a point estimate.
- **Predictions:** (a) right, (b) wrong, (c) right.
- **Spread of pool-verify-v1's F:**
  - over Cal.com runs 1–3: 51.3–58.3%;
  - over dev runs 1 and 2r: 35.6% and 48.0%.
- **Spend:** $5.31 of the $15 cap.
- **Void and unfinished runs:**
  - Dev run 2 was void. The relay session's 2-hour background limit killed it, and it was restarted once as 2r.
  - Dev run 3 stopped at the cap and was not restarted, as 061 requires.

## Verdict
- **Outcome:** tie. The pre-registered run gave a result, and pool-verify-v1 beat its own secondary. Later analysis with intervals (decisions 066 and 069b) reads it against the tools as a statistical tie with Greptile and Cursor, not as "3rd".
- **Call:** sound. The plan was frozen first, the cap order was followed, and a cap stop was not restarted. Its own limits section already said the run-to-run spread was larger than the margins.

## Links
- Plan: [D061](D061-test-plan.md). Framing of the result: [D063](D063-results-page.md), [D066](D066-results-wording.md).
- Commands: [docs/bench/test-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/test-plan.md).
