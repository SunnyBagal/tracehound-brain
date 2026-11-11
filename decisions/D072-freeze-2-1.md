---
id: D072
title: review-freeze-2.1, the Action's install step only
date: 2026-10-27
kind: product
lane: product
outcome: win
call: sound
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#072--review-freeze-21-the-actions-install-step-only-2026-10-27
pr: 68
---

# D072 · review-freeze-2.1

## Decided
`review-freeze-2.1` is `review-freeze-2` (`0aeef06`) plus one commit (`86fd5d7`) that changes only the composite action's install step.
- **New install step:** `actions/setup-node@v4`, then `npm install -g pnpm@11.20.0` in the action's own checkout.
- **Result:** nothing reads the reviewed repo's `package.json` any more.
- **Unchanged:** review code, prompts, config and bench scripts are identical, so every published result still describes freeze-2.

## Why, at the time
In the seeded test, every TraceHound run on the Linea repo failed before any model call with `Multiple versions of pnpm specified`. `pnpm/action-setup` also read the reviewed repo's `packageManager` pin, so any repo pinning another pnpm could not run TraceHound.

## Expected
Not recorded as a prediction. The proof was set up as a CI matrix: six root `package.json` cases (pnpm 9 / 10 / 11, yarn, npm, none).

## What happened
- **Before and after:** the matrix (`action-install-matrix.yml`) fails for pnpm@9 on the old action and passes all six after the fix (#68).
- **Scope:** `git diff 0aeef06 86fd5d7 --stat` lists only `action.yml`.
- **Pin:** the review workflow pins the 2.1 commit.

## Verdict
- **Outcome:** win. 6 of 6 cases install, and none of the reviewer's code moved.
- **Call:** sound. It was the smallest possible change, with proof that runs both ways.

## Links
[lessons](../lessons.md) (2026-10-28, a re-pin that didn't reach an open PR) · [D068](D068-partial-reviews.md)
