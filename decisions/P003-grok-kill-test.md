---
id: P003
title: Grok kill test — every audit finding cites a real file and line
date: 2026-10-15
kind: process
lane: chat
outcome: win
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: none
pr: 
---

# P003 · Grok kill test

## Decided
Every Grok audit finding must cite a file and line that exist and say what Grok claims. If it invents one, drop Grok and rerun the audit in a fresh ChatGPT chat.

## Why, at the time
An auditor that hallucinates evidence is worse than none.

## Expected
Not recorded.

## What happened
First audit (2026-10-15, on main `f610efa`): every citation checked out. Its findings:
1. Golden set extended with the recorded tools' own comments, which favours them.
2. One judge, small n.
3. The verify threshold was chosen on dev.
4. The judge sees no diff.
5. The prompt asks only for merge-blocking defects.
6. Arithmetic correct; no overclaims, no tuning leaks.

## Verdict
- **Outcome:** win. Finding 1 is the most important threat on record ([D054](D054-review-benchmark-scorer.md)).
- **Call:** sound. Keep running the test on every audit; one pass proves little.
