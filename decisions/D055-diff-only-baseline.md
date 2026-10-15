---
id: D055
title: Diff-only baseline reviewer, review-diff-v1
date: 2026-10-15
kind: experiment
lane: benchmark
outcome: n/a
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#055--diff-only-baseline-reviewer-review-diff-v1
pr: 23
---

# D055 · Diff-only baseline

## Decided
The simplest reviewer first: the PR's title, description and diff only, asking for merge-blocking defects. Super and Nano.

## Why, at the time
Every later change needs a baseline on the same scorer.

## Expected
Not recorded.

## What happened
- Super: P 25.6% (32/125), R 43.2% (32/74), F 32.2%.
- Nano: F 23.4%.
- Recall is at CodeRabbit's level (43.2%); precision is the gap.

## Verdict
- **Outcome:** n/a (a baseline)
- **Call:** sound. It located the problem (precision) for every later job.

## Links
- [results](../results.md)
