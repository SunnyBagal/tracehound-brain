---
id: D054
title: Review benchmark scorer — golden comments, recorded tools, our own judge
date: 2026-10-15
kind: scorer
lane: benchmark
outcome: win
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#054--review-benchmark-golden-comments-recorded-tool-reviews-and-our-own-judge
pr: 22
---

# D054 · Benchmark scorer

## Decided
Score reviewers against the public golden comments (`ai-code-review-evaluations`, pinned at `3f2c8ab`), with judge `bench-judge-v1` (Nemotron Super, temperature 0, one judge). Low goldens and duplicates count as neutral; counts are micro-averaged. Dev set: sentry, grafana, keycloak, discourse. Test set: Cal.com, kept blind.

## Why, at the time
Five commercial tools' reviews are recorded on the same PRs, which gives an external comparison no hand-made benchmark could.

## Expected
That our judge would reproduce the published tool ranking.

## What happened
Same F ranking as Augment's published table; our judge gives higher precision for all five tools.

## Verdict
- **Outcome:** win. A working, reproducible scorer.
- **Call:** sound, with known threats that are still open:
  1. The golden set was extended by Augment using the recorded tools' own comments. A valid new finding from TraceHound counts as a false positive. This is the biggest threat to any "beats X" claim. Answer: [059](../NOW.md), augmented precision for every reviewer.
  2. One judge with unmeasured error; small n (74 dev; the test set is smaller).
  3. The judge sees no diff, and the goldens have no location, so matching depends on wording.
  4. The goldens are broader than "merge-blocking", which our reviewer prompt asks for. That can cost recall.
- **What we'd do differently:** measure judge agreement (a second judge) at the scorer stage, not at the end.

## Links
- Grok's audit raised threats 1–4 ([P003](P003-grok-kill-test.md)).
