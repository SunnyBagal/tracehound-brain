# The benchmark: how we measure

Repo decision: [054](../decisions/digest.md#054--review-benchmark-golden-comments-recorded-tool-reviews-and-our-own-judge). Validation doc: `docs/bench/scorer-validation.md`.

## Dataset

- **Source:** the 50-PR code review set first published by Greptile, with corrected golden comments from `github.com/ai-code-review-evaluations/golden_comments`, pinned at `3f2c8ab794b4ed71c099cbf0a7670555e90906dc` (21 Nov 2025).
- **Size:** 5 repos × 10 PRs, 137 golden comments, 97 of them Medium or higher.
- **Recorded tool reviews:** five commercial tools' real review comments sit in per-tool forks (`<tool>-<repo>`) in the same GitHub org. All 50 PRs map for every tool.
- **Not copied into the repo:** the dataset has no licence file, and Cal.com is AGPL. All data lives in `~/Projects/tracehound-bench`.

| Split | Repos (language) | PRs | Golden ≥ Medium |
|---|---|---|---|
| **Dev** (tune freely) | sentry (Python) 21, grafana (Go) 16, keycloak (Java) 18, discourse (Ruby) 19 | 40 | **74** |
| **Test** (blind, run once after the freeze) | Cal.com (TypeScript) | 10 | small. Results only after the 23 Oct freeze |

## Scoring rules (from the dataset README)

- A judge maps each review comment to one golden comment or to none.
- **TP:** matches a golden comment of Medium or higher. **FP:** matches nothing.
- **low:** matches a Low golden comment, and counts as nothing. **dup:** a second comment matching the same golden comment, also counts as nothing.
- **FN:** a Medium-or-higher golden comment that nothing matched.
- Precision = TP/(TP+FP), recall = TP/(TP+FN), F = harmonic mean. All are micro-averaged over summed counts.
- JUDGE_FAILED PRs are left out and counted. So far that has happened 0 times.

## The judge, `bench-judge-v1`

Nemotron Super, temperature 0, reasoning on, one request per PR, prompt sha256 `23de4e9e…058460`. stdout gives counts only; per-comment detail is written outside the repo.

**Validation (054):** scored on all five tools, our judge reproduces the order in Augment's published table exactly (Augment > Cursor > Greptile > CodeRabbit > Copilot). It gives every tool higher precision than the published figures, by 2 to 13 points.

## Why our own judge

The published numbers disagree. On the same 50 PRs:
- Greptile reports an 82% catch rate for itself and 44% for CodeRabbit.
- Tenki reports recall of 36% and 29%.
- Augment reports F of 45% and 39%.

Scoring every reviewer, ours included, with one judge is the only comparison we can stand behind.

## Blindness rules

- No session that builds or tunes the reviewer reads Cal.com golden comments, the tools' Cal.com review comments, or per-comment Cal.com score detail. **Counts are fine.**
- The reviewer is frozen (tag) before its one Cal.com run.
- The chat that plans the work has seen Cal.com tool counts but no content.
- `fetch-repos.ts` refuses any repo outside the four dev repos.

## Known threats to validity (open)

1. **Golden set extended with the recorded tools' own comments** (Augment did this). A valid new finding from TraceHound that none of the five tools made counts as a false positive. This biases precision against us. Fix planned: **060**, augmented precision (the same judge rules on every unmatched comment of every reviewer against the diff).
2. **One judge, with unmeasured error.** A second judge is planned in 060.
3. **Small n:** 74 dev and fewer on test. Gaps of a few points are noise.
4. **No diff for the judge, and golden comments have no location.** Matching depends on wording.
5. **Scope mismatch:** our prompt asks for merge-blocking defects, while the golden set is broader, which can cost recall.
6. **Selection on dev:** each new variant is compared on the same 40 PRs. Picking the best favours luck. The single blind Cal.com run is the guard.
7. **Contamination:** the PRs have been public since 2025, so models may have seen them. This applies to every tool.
8. **Ledger prices are third-party estimates**, not confirmed in the Nebius console. Costs are estimates.
