---
id: D068
title: Nemotron 3.5 default with partial reviews on failed chunks
date: 2026-10-22
kind: product
lane: product
outcome: n/a
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#068--nemotron-35-default-with-partial-reviews-on-failed-chunks
pr: 57
---

# D068 · 3.5 default, partial reviews

## Decided
The Action's `model` input defaults to `nvidia/Nemotron-3.5-Super-VL`.
- **Failed chunks:** a failed reviewer chunk no longer throws away the whole review. Successful chunks are pooled and verified as usual.
- **Footer:** it shows the failed/total chunk count.
- **ReviewBench adapters:** they report `status=ok partial=<failed>/<chunks>`.
- **Unchanged:** fixes stay on 3 Super (not measured on 3.5).

## Why, at the time
- The benchmark in [D067](D067-nemotron-35.md) kept every successful chunk's comments, while the Action discarded everything when any chunk failed.
- So the product did not match what was measured. All-or-nothing would have posted nothing on about 1 in 6 PRs.

## Expected
Not recorded. No model call or live check was made; the tests use synthetic stage output.

## What happened
- The chat caught the bench-vs-product mismatch, and codex built the change (codex-12). #57 merged as `0aeef06`, which became `review-freeze-2`.
- Later work found that these chunk failures were mostly empty replies ([empty replies](../project/empty-replies.md)).

## Verdict
- **Outcome:** n/a (product default).
- **Call:** sound. The product now does what was measured, and it doesn't add an unmeasured fallback model.

## Links
[D067](D067-nemotron-35.md) · [D069](D069-v2-calcom.md) · [D073](D073-recovery-ab.md)
