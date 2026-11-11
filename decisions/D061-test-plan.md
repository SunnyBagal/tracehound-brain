---
id: D061
title: Pre-registered plan for the one Cal.com run, and the code it needs
date: 2026-10-16
kind: process
lane: benchmark
outcome: win
call: sound
judged_by: artu1 (from D062 and the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#061--pre-registered-plan-for-the-one-calcom-test-set-run
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

## What happened
Executed as [D062](D062-calcom-result.md) from the tag `review-freeze-1`: Cal.com run 1 (n = 23) gave pool-verify-v1 F 58.3% against review-diff-v1's 53.8%; predictions (a) right, (b) wrong, (c) right; spend $5.31 of the $15 cap. Dev run 2 was void and restarted once as 2r (the 061a amendment); dev run 3 stopped at the cap and was not restarted, as the plan required. Source: [docs/bench/test-results.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/test-results.md).

## Verdict
- **Outcome:** win. The plan did its job: one frozen, pre-registered headline run, scored as written, under the cap, with its spread runs reported beside it. (Whether pool-verify-v1 itself did well is D062's verdict: a tie with the tools inside the intervals.)
- **Call:** sound. Freezing before the run, fixing the predictions and the cap order in advance, and allowing one infrastructure rerun only kept the result clean; the one rerun (dev 2r) followed the amendment.
- **Rules out:** tuning on Cal.com after this run. (The one later Cal.com run, [D069](D069-v2-calcom.md), was a separately approved blind second look, not a rerun of this one.)

## Links
- Repo: [decision 061](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#061--pre-registered-plan-for-the-one-calcom-test-set-run); commands [docs/bench/test-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/test-plan.md).
- Result: [D062](D062-calcom-result.md); framing [D063](D063-results-page.md).
