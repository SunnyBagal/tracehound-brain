---
id: D057
title: Repository context for the reviewer, review-ctx-v1
date: 2026-10-15
kind: experiment
lane: benchmark
outcome: tie
call: flawed
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#057--repository-context-for-the-reviewer-review-ctx-v1
pr: 26
---

# D057 · review-ctx-v1

## Decided
review-diff-v1 plus a deterministic, language-agnostic context block per chunk: the changed files at the head, plus `git grep -w` references to changed names, within 40,000 characters. Super, 40 dev PRs, $3 cap.

## Why, at the time
"Anything that fixes precision has to see more than the diff" (handoff, 2026-10-15). It had to work for all four dev languages, not just TypeScript.

## Expected
Higher precision without losing recall. No threshold was written.

## What happened
- P 36.2% (21/58), R 28.4% (21/74), F 31.8%, against diff-v1's F 32.2%.
- False positives fell from 93 to 37; true positives fell from 32 to 21.
- References reached the model in only 12 of 48 chunks (147 of 1,088 selected). Whole changed files were dropped in 33 chunks, a cut step the builder added because the brief didn't cover the case.
- One discourse PR got 66 comments; 64 matched Low goldens and scored nothing.
- Cost: $0.857.

## Verdict
- **Outcome:** tie. Context made Super more conservative, not sharper.
- **Call:** flawed in the brief, not the idea. The cut order dropped references first, which was the thing under test. And everything landed in one commit, so the prompt freeze rests on the builder's word.
- **What we'd do differently:** protect the variable under test in any budget, and push a freeze commit before the first model call. Both are applied in [D058](D058-verify-ctx.md).

## Links
- [results](../results.md), [lessons](../lessons.md)
