---
id: D066
title: Results wording fixes, all-50 table, levers tried (docs only)
date: 2026-10-20
kind: process
lane: benchmark
outcome: n/a
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#066--results-wording-fixes-all-50-table-levers-tried-docs-only
pr: 52
---

# D066 · Results wording fixes

## Decided
- **New public claim:** on 10 blind PRs, TraceHound is statistically tied with Greptile and Cursor, and it is 4th of 6 across all 50 PRs. "3rd of six" is no longer used, and per-run ranks are labelled point estimates.
- **Audit fixes:** the cc-01 audit's five wording fixes go in as dated notes. The old text of 061 and 063 is unchanged.
- **New tables:**
  - an all-50 table with two declared TraceHound rows, under both judges;
  - one table of the precision levers tried (056–059, 064, 065).
- **Viewer:** the `/benchmark` tables must equal results.md, enforced by a test.

## Why, at the time
- The cc-01 audit ([R6](../research/R6-cc01-audit.md)) found no wrong number but five overclaims.
- 064 and 065 had both lost.
- Five dev runs of the adopted reviewer spread 12.4 F points.

The public page had to say all of this before anything else was built on it.

## Expected
Not recorded (docs only, no new results).

## What happened
- **All 50 PRs:** row (i) had v1 F 41.2% and x1 48.1%, 4th of 6 under both judges.
- **Interval statement:** no gap to another tool is outside its 95% interval. The CodeRabbit gap is the clearest, ahead in 96% of resamples, but its interval still includes 0.
- **Temperature:** all runs used temperature 0, while NVIDIA recommends 1.0. This is recorded as a limit.

Source: [docs/bench/results.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/results.md).

## Verdict
- **Outcome:** n/a (docs).
- **Call:** sound. It corrected [D063](D063-results-page.md)'s framing with dated notes instead of rewriting history, and it chose the headline row by pre-registration, not by which row looked better.
- **What it rules out:** any rank claim without an interval in public text.

## Links
[D063](D063-results-page.md) · [D062](D062-calcom-result.md) · [results](../results.md) · [benchmark](../benchmark/benchmark.md)
