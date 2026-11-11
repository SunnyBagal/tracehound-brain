---
id: D075
title: Pre-registered verified quality of 074's A vs E findings
date: 2026-11-08
kind: experiment
lane: benchmark
outcome: loss
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#075--pre-registered-verified-quality-of-074s-a-vs-e-findings-2026-11-08
pr: none (branch bench/hard-chunks-074, merged as 66e3d8a)
---

# D075 · Verified A vs E

## Decided
Take [D074](D074-hard-chunks.md)'s records, run verify-ctx-v1 unchanged on each arm's findings, and score them with one judge (bench-judge-v1) against the dev goldens. No reviewer call. The cap was $1.50.

The claim wording for every outcome was fixed in advance (devin).

## Why, at the time
074 showed that E returns more findings after an empty reply. It did not show whether they are right.

## Expected
- **PASS (adopt E):** P(E) ≥ P(A) and TP(E) − TP(A) ≥ 3, both with "keep or VERIFY_FAILED" and with "keep only". "Adopt" meant only that E goes on to a full-dev-set check.
- **INCONCLUSIVE:** any JUDGE_FAILED review with comments.

## What happened
Source: [docs/decisions.md 075 result](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#075--pre-registered-verified-quality-of-074s-a-vs-e-findings-2026-11-08).
- **The run:** complete, $0.49090. One judge call failed with HTTP 529 ("worker overloaded"), which makes it **INCONCLUSIVE** by the rule.
- **Diagnostics only:**

  | arm | found | kept | TP | FP |
  |---|---|---|---|---|
  | A | 30 | 12 | 5 | 3 |
  | E | 135 | 20 | 6 | 8 |

  The verifier dropped 85% of E's findings.
- **The arithmetic bound:** whatever the failed review would have scored, TP(E) − TP(A) ≤ 1, so the gate fails in every completion.
- **Closing (owner's ruling, 2026-11-09):** E dropped; the defaults stay at the append retry plus the lenient parser. Empty replies are a known limitation.

## Verdict
- **Outcome:** loss for E. The result is formally INCONCLUSIVE, but E fails the gate under every possible completion.
- **Call:** sound. The design was cheap and quality-first, the wording was fixed before the run, and no unregistered rerun was made.
- **Minor:** unlike 061, it pre-registered no rerun for an infrastructure failure. The bound made that moot here.
- **What it rules out:** E's extra volume as a source of kept true positives.

## Links
[D074](D074-hard-chunks.md) · [empty replies](../project/empty-replies.md) · budget line: [docs/budget.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/budget.md)
