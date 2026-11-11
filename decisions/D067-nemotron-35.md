---
id: D067
title: Nemotron 3.5 Super VL as the reviewer and verifier model (dev only), adopted on 2 runs
date: 2026-10-22
kind: experiment
lane: benchmark
outcome: win
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#067--nemotron-35-super-as-the-reviewer-and-verifier-model-dev-only-adopted-on-2-runs
pr: 56
---

# D067 · Nemotron 3.5 Super VL

## Decided
Swap only the reviewer and verifier model to `nvidia/Nemotron-3.5-Super-VL` in pool-verify-v1. Prompts, temperature 0, reasoning on, `max_tokens` and the judges all stay the same.
- **Plan:** the plan, rule, baseline and a 2-run fallback were frozen in `docs/bench/dev-3.5-plan.md` before the first call.
- **First run 7:** 35 calls got HTTP 404 because Token Factory serves the 3.5 id only from us-north1.
- **Fix:** #55 routes that one id there.
- **Amendment 1:** voided run 7 ($0) and rewrote the stop rule. It was pushed before any successful 3.5 call.

## Why, at the time
The Action's default model had to be decided before freeze-2 (24 Oct).

## Expected
All three conditions, against the 3 Super 5-run means: F ≥ 45.28, P ≥ 40.52, x1 F ≥ 51.29. If run 9 could not finish, the 2-run fallback would decide.

## What happened
Source: [docs/bench/dev-3.5.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/dev-3.5.md).
- **Runs:** runs 7r and 8 completed; the $8 cap stopped run 9 at PR 24.
- **Means:** v1 P 54.8% / R 46.6% / F 50.4%, and x1 F 55.1%. All three conditions pass.
- **Paired bootstrap of the F difference:**
  - v1: +8.1 [+1.4, +14.4];
  - x1: +3.9 [−4.2, +11.1], which includes 0.
- **Where the gain is:** precision, not recall.
- **Cost:** median $0.063 per PR against $0.031.
- **Failures:** 7 of 40 PRs per run had a chunk that returned invalid JSON twice (3 Super: 0–1), and 79 of 624 calls hit the 32,000-token limit.
- **Spend:** $7.98.

## Verdict
- **Outcome:** win by the pre-set rule. It is the only lever since 059 with an interval excluding 0, under v1 only.
- **Call:** sound. The rule and fallback were frozen first, and the cap was not raised to finish run 9.
- **Cost of the gain:** the chunk failures it brought became the reliability work of [D068](D068-partial-reviews.md), [D073](D073-recovery-ab.md) and [D074](D074-hard-chunks.md). Their root cause is in [empty replies](../project/empty-replies.md).

## Links
Routing fix PR #55 · [D068](D068-partial-reviews.md) · [D069](D069-v2-calcom.md)
