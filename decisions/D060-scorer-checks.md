---
id: D060
title: Scorer checks — second judge (Qwen3-235B) and augmented precision
date: 2026-10-16
kind: scorer
lane: benchmark
outcome: n/a
call: sound
judged_by: chat
judged_on: 2026-10-16
repo_entry: pending merge of PR #31
pr: 31
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

## What happened (freeze `b98f014` before the first call; $0.635)
- **The judges largely agree.** Same label on 888 of 1,130 comments (78.6%). Kappa 0.809 on the outcome class. Kendall tau-b 0.891 on the F-ranking of 11 runs, and the top 5 ranks are identical.
- **Qwen is more lenient overall.** pool-verify-v1 F: 35.6 under v1, **44.6 under x1** (P 47.7, R 41.9). Its rank is 5th of 11 under both judges, behind Augment, Cursor, Greptile and CodeRabbit and ahead of Copilot. Under x1, CodeRabbit is at 47.4, 2.8 points away.
- **Self-preference is not visible.**
  - Precision gap (v1 minus x1): TraceHound runs −4.5 on average, tools −6.6. So v1 is 2.1 points relatively kinder to us, which is the predicted direction, but small.
  - For pool-verify-v1 the gap is −11.6, meaning v1 is *harsher* on it than x1.
- **The validity check is weak.**
  - The two validity judges agree on 70.7% (kappa 0.395).
  - Qwen calls 265 of 324 FPs valid.
  - Super calls 0 of pool-verify-v1's FPs invalid. That is circular: the same model's verifier had already kept them.
  - Augmented precision (Super) is 84–93% for the top four tools and 88.9% for pool-verify-v1. Copilot is at 51.4%.
  - Read: the golden set undercounts valid findings for everyone. This is not a corrected precision.

## Verdict
- **Outcome:** n/a (report only). The headline stays bench-judge-v1.
- **Call:** sound. It cleared the self-preference worry with numbers, confirmed the ranking under a second model family, and showed the validity check is too lenient to use as a score.
- **What we'd do differently:** design validity-v1 with a stricter rubric (ReviewBench's "true, relevant, non-trivial", [R2](../research/R2-market-benchmarks.md)). Exclude Super from judging Super-verified comments.
