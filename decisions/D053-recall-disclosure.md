---
id: D053
title: Disclosure of who saw Recall's real bugs
date: 2026-10-14
kind: process
lane: benchmark
outcome: n/a
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#053--disclosure-who-saw-recalls-real-bugs-and-what-finder-results-on-recall-may-claim
pr: 21
---

# D053 · Recall disclosure

## Decided
Record in the repo who saw Recall's three real bugs and when, without describing them. Every finder, reproduce or repair result on those bugs carries a pointer to 053 and is reported as a count.

## Why, at the time
The finder's rule families were chosen by a session that knew of the bugs, so a finder hit is not a blind discovery. Readers of the results need to see that.

## Expected
n/a

## What happened
Recall results are no longer a headline. Cal.com became the only blind test set.

## Verdict
- **Outcome:** n/a
- **Call:** sound. It cost a headline but kept every later claim defensible.
- **What we'd do differently:** decide blindness rules before any session hears about a bug. Here it was reconstructed after the fact.

## Links
- Never describe the bugs here either. Never open Recall PRs #1 or #2.
