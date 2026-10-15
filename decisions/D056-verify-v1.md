---
id: D056
title: Verification pass, verify-v1 (diff only, confidence 1–5)
date: 2026-10-15
kind: experiment
lane: benchmark
outcome: loss
call: flawed
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#056--verification-pass-on-review-findings-verify-v1
pr: 24
---

# D056 · verify-v1

## Decided
A second Super pass over each diff-v1 finding, seeing only the diff, giving a confidence from 1 to 5. The threshold was picked by a rule set before the run.

## Why, at the time
Precision was the gap, and a verifier is the standard fix.

## Expected
That higher confidence would mean a true finding.

## What happened
- The pre-set rule picked threshold 1, which keeps everything.
- Confidence barely separates TP from FP: 6 of 32 TPs got confidence 1.
- Threshold 5 (chosen after the fact, so not valid): P 44.4%, R 21.6%.

## Verdict
- **Outcome:** loss
- **Call:** flawed. The verifier saw nothing the generator hadn't. And self-reported LLM confidence is known to be poorly calibrated.
- **What we'd do differently:** give the verifier information the generator lacked, and ask for a decision rather than a number. That is [D058](D058-verify-ctx.md).

## Links
- [lessons](../lessons.md)
