---
id: P008
title: ReviewBench submission — pre-registered config choice and blindness
date: 2026-10-19
kind: experiment
lane: chat
outcome: pending
call: too-early
judged_by:
judged_on:
repo_entry: none (docs/bench/reviewbench.md once rb-01 merges)
pr:
---

# P008 · ReviewBench pre-registration

Written **before** any TraceHound output has been run on any ReviewBench PR (19 Oct 2026, 18:20 IST).

## Blindness
- Nobody on TraceHound (chat, relay, Codex, Claude B, cloud sessions, Grok) opens ReviewBench's `golden/` directory or its judge/classifier prompts, before or after the final run. Tuning on the full set is allowed by ReviewBench's rules; **we don't do it.**
- The only feedback we use before the final run is the portal's **25-PR test run**, a subset of the 219.

## Candidate configs (all use existing, frozen prompts; nothing new)
1. `pool-verify-v1`: what the Action ships.
2. `pool-raw`: the same generators with no verifier.
3. `pool-raw-x2`: pool-raw twice, union, deduplicated.

## Selection rule
- Run each config **once** on the 25-PR test run (3 runs).
- Pick the highest **grounded F1**. If two are within 2.0 points, pick the cheaper one (the order is pool-verify-v1, pool-raw, pool-raw-x2).
- Then do **one** final run (219 × 3 rounds) with the chosen config. No second final run unless the first fails for infrastructure reasons, and that would be stated.

## Why recall-heavy configs are in scope
ReviewBench's grounded precision ignores findings that don't match the golden set, so its grounded F1 is driven by recall. Every leaderboard entry has P 84–90%. This is their published methodology, not a loophole. We will publish **grounded and augmented** numbers side by side, and say plainly that the grounded metric favours reviewers that post more.

## Expected (written before any run)
- Grounded recall ≥ 16% would put us above Greptile (27.2) and Cursor (17.3). We guess the 25–30% range (F ≈ 38–44), not promised.
- pool-raw is expected to beat pool-verify-v1 on grounded F1 and lose on augmented precision.

## Budget
About $26 of Nebius for the final run plus about $3 for the three test runs. That needs a top-up (Sunny, 19 Oct).
