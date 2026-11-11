---
id: D071
title: Nemotron 3 Ultra on dev via the free NVIDIA API (report only)
date: 2026-10-24
kind: experiment
lane: benchmark
outcome: n/a
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#071--nemotron-3-ultra-on-dev-via-the-free-nvidia-api-report-only-2026-10-24
pr: 63
---

# D071 · Nemotron 3 Ultra on dev (report only)

## Decided
Add an opt-in second provider in `src/llm/client.ts`.
- **When it applies:** only with `TRACEHOUND_NVIDIA_API=1` and the exact id `nvidia/nemotron-3-ultra-550b-a55b`. Requests then go to NVIDIA's free API (36 requests per minute, 300 s timeout, ledgered at $0).
- **The runs:** dev runs 10–11 of pool-verify-v1 at `review-freeze-2`, with only the model swapped.

## Why, at the time
Ultra is too expensive on Token Factory, and the free route made a look at it cost $0.

## Expected
Not recorded beyond "report only": nothing would be adopted from it.

## What happened
Source: [docs/bench/dev-ultra.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/dev-ultra.md).
- **pool-verify-v1:** v1 F 42.3 and 51.9 (mean 47.1). That shows no interval-backed difference from 3 Super (mean 42.3) or 3.5 (mean 50.4).
- **The one interval excluding 0:** pool-raw Ultra − 3.5 = −10.2 [−17.7, −3.2].
- **Operations:** run 11 had 57 HTTP 429s in 361 requests and one failed chunk.
- **Review:** Devin reviewed it on #63.

## Verdict
- **Outcome:** n/a (report only). Nothing was adopted, and the Action stays on 3.5.
- **Call:** sound. It cost $0, every Token Factory path stayed unchanged under tests, and the frozen limits were not raised after the smoke.
- **What it rules out:** a bigger Nemotron as an easy F gain on dev.

## Links
[D067](D067-nemotron-35.md) · the 300 s timeout this route keeps on purpose: [300 s cutoff](../project/300s-transport-cutoff.md)
