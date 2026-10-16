---
id: D059
title: Pooled candidates filtered by the context verifier, pool-verify-v1
date: 2026-10-16
kind: experiment
lane: benchmark
outcome: pending
call: too-early
judged_by: 
judged_on: 
repo_entry: pending
pr: 
---

# D059 · pool-verify-v1

## Decided
Pool review-diff-v1's findings with review-ctx-v1's findings that diff-v1 didn't already make (same path within 3 lines counts as already made). Filter the pool with verify-ctx-v1, unchanged. diff-v1 findings reuse their 058 verdicts, so only the ctx-only findings get new verifier calls. No new prompt and no new reviewer run. Chosen by Sunny on 2026-10-16 over doing the scorer-fairness job first.

## Why, at the time
- 058 showed the verifier separates true from false findings (it kept 69% of TPs and 33% of FPs), but diff-v1 alone supplies only 32 TPs.
- ctx-v1 finds some defects diff-v1 misses, so a wider pool should raise recall after filtering.
- It costs well under $1 and reuses frozen pieces.

## Expected (pre-registered in the brief, before any run)
- Adopt only if F ≥ 35.0% on the 40 dev PRs. Otherwise diff-v1 stays the default.
- All 143 reused 058 verdicts must match their findings, or the job stops.
- Freeze commit (rules and pool builder) pushed before the first verifier call. Nothing under `packages/analyzer/src/review/` changes.
- Known bias: this is the fourth variant tested on the same dev set, so whichever wins is favoured by selection. Only the Cal.com run confirms it.

## What happened
Pending.

## Verdict
Pending.

## Links
- Brief: `~/Projects/tracehound-ops/briefs/059-pool-verify.md` (saved in the repo as `docs/prompts/059-pool-verify.md` by the job). Previous: [D058](D058-verify-ctx.md).
