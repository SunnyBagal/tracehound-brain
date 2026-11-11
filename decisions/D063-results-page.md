---
id: D063
title: Benchmark results page and its framing (docs only)
date: 2026-10-18
kind: process
lane: benchmark
outcome: n/a
call: flawed
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#063--benchmark-results-page-and-its-framing-docs-only
pr: 39
---

# D063 · Results page

## Decided
One public page, [docs/bench/results.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/results.md), with counts copied from the existing result files, plus a short README section. The first paragraph said TraceHound ranked 3rd of six on the blind test under the scorer of record and 5th of six on dev. It also said the spread is larger than the margins, so single-run ranks are approximate. No model call.

## Why, at the time
062's numbers were spread over four result files and nine decisions. Nothing summarised them for a reader outside the project.

## Expected
Not recorded. The entry rejected a "beats Greptile" headline because the run-to-run spread was larger than the margin.

## What happened
- The cc-01 audit ([R6](../research/R6-cc01-audit.md)) recomputed every number and found none wrong.
- It did find five wording overclaims on this page. 066 fixed them and retired "3rd of six" as a ranking ([D066](D066-results-wording.md)).

## Verdict
- **Outcome:** n/a (docs).
- **Call:** flawed. The facts behind the overclaims were already in the repo when the page was written:
  - the tools' Cal.com scores had been in the repo since 054;
  - run 1 was one of three runs;
  - the judge is the same model as the reviewer.
- **What we'd do differently:** state every rank as a point estimate with its interval in the first draft. Don't publish a rank that the run-to-run spread can reverse.

## Links
[D062](D062-calcom-result.md) · [D066](D066-results-wording.md) · [results](../results.md)
