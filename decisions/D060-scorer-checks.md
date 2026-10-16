---
id: D060
title: Scorer checks — second judge (Qwen3-235B) and augmented precision
date: 2026-10-16
kind: scorer
lane: benchmark
outcome: pending
call: too-early
judged_by: 
judged_on: 
repo_entry: pending
pr: 
---

# D060 · Scorer checks

## Decided (Sunny, 2026-10-16)
- **Second judge, bench-judge-x1:** `Qwen/Qwen3-235B-A22B-Instruct-2507`, with the same prompt as bench-judge-v1. It is a different model family, so it can reveal self-preference: the reviewer and bench-judge-v1 are both Nemotron Super.
- **Augmented precision:** every comment judged FP is checked for validity against the diff by Super and by Qwen (validity-v1), for us and all five tools alike.
- Report only. bench-judge-v1 stays the scorer of record. No adoption or headline changes. Cap $5.

## Why, at the time
- One judge whose error has never been measured; the same model family reviews and judges.
- Grok's finding 1: the golden set was extended with the five tools' own comments, so a new reviewer's valid finding counts as an FP.
- 059 cleared its bar by 0.6 points, so judge noise matters.

## Expected (pre-registered in the brief)
- Agreement share, Cohen's kappa, Kendall's tau over the run ranking.
- Self-preference gap: precision under v1 minus x1, mean for TraceHound runs vs the tools.
- Augmented precision three ways (Super valid / Qwen valid / both valid). If the validity check doesn't finish, none is reported.
- pool-verify-v1's F under x1 is reported as a fact that changes nothing.

## What happened
Pending.

## Verdict
Pending.
