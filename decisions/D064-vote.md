---
id: D064
title: Vote across runs (dev only), a loss
date: 2026-10-19
kind: experiment
lane: benchmark
outcome: loss
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#064--vote-across-runs-dev-only-a-loss
pr: 46
---

# D064 · Vote across runs

## Decided
Run dev runs 4–6 with the 061 runner (cap $8.50). Then keep only the findings that recur across five independent runs (vote-k3of5 and other variants). The plan was pushed (`57f0b91`) before any model call.

## Why, at the time
pool-verify-v1's gap to the top two tools on Cal.com was precision. Dev F had moved 12.4 points between two runs at temperature 0 ([D062](D062-calcom-result.md)). The idea was that a finding that recurs is more often real.

## Expected
The pre-registered rule ([vote-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/vote-plan.md)): adopt a variant only if it beats the 5-run baseline mean by 5 F points, meets the precision condition, and also wins under bench-judge-x1.

## What happened
Source: [docs/bench/vote.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/vote.md).
- **vote-k3of5:** v1 F 47.8% against a baseline mean of 42.3% (+5.5), but x1 F 50.4% against an x1 mean of 51.3%. It fails.
- **Baseline spread over five runs:** F 35.6–48.0% under v1. Run 1, the run the reviewer was chosen on, was the lowest.
- **Cost:** a 5-run vote would cost about 5× per PR. The job spent $5.41.

## Verdict
- **Outcome:** loss. pool-verify-v1 stays.
- **Call:** sound. The rule was frozen first, and the x1 condition stopped a v1-only win from being adopted.
- **What it rules out:** voting across runs as a precision lever at this cost. It also showed that every later comparison has to use 5-run means, not single runs.

## Links
Brief: `docs/prompts/064-vote.md` in the repo. Next: [D065](D065-evidence-gate.md).
