---
id: P004
title: Pitch the honest benchmark and the live product, not "beats Greptile"
date: 2026-10-15
kind: process
lane: chat
outcome: rejected
call: too-early
judged_by: Sunny
judged_on: 2026-10-18
repo_entry: none
pr: 
---

# P004 · Target reset (proposed, not agreed)

## Proposed
Stop treating "beat Greptile and CodeRabbit on Cal.com" as the goal. Pitch TraceHound as an open, honestly measured PR reviewer on Nemotron, running live on real PRs, and put the remaining time into the demo.

## Why
- Dev F is 32.2%; Greptile's dev F is 49.3%. A 17-point gap is unlikely to close in two precision rounds, and the test set is small.
- CodeRabbit's dev F (41.0%) is the realistic ceiling.
- The judging criteria are viability, technical implementation, design, impact and idea quality. Leaderboard rank is not one of them.
- The demo decides viability (pass/fail) and most of design.

## Status
**Rejected by Sunny, 2026-10-18.** "Beats" may appear, and the goal is the **top 2 on a blind test**. Chat's condition, accepted as framing: every "beats" claim names the test, the run and the scorer (for example "on our pre-registered blind test, F 58.3 vs Greptile 50.0"). Plan: 064 voting across runs (dev only) → 065 one pre-registered Cal.com run of the adopted variant.
