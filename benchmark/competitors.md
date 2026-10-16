# The five recorded reviewers, and how we compare

All numbers come from our judge on the **dev set** (40 PRs, n = 74), from `docs/bench/scorer-validation.md` and `docs/bench/baseline-diff.md`. The "what it is" lines describe the tool only as far as this benchmark shows it. We never ran these tools ourselves; their comments were recorded by the dataset authors in 2025, so they reflect each tool's 2025 version.

| Tool | What it is here | Bot login |
|---|---|---|
| **Augment** | Augment Code's PR reviewer. Augment also maintains the corrected golden set and published a write-up (updated 18 Jun 2026) | `augmentcode[bot]` |
| **Cursor** | Cursor's PR reviewer, Bugbot | `cursor[bot]` |
| **Greptile** | AI PR reviewer. Greptile first published this 50-PR set | `greptile-apps[bot]` |
| **CodeRabbit** | AI PR review bot | `coderabbitai[bot]` |
| **Copilot** | GitHub Copilot code review | `Copilot` |

## Dev scoreboard, with comment volume

| Reviewer | P | R | F | comments/PR | TP/PR | FP/PR | share of comments wrong |
|---|---|---|---|---|---|---|---|
| Augment | 75.9% | 55.4% | **64.1%** | 1.73 | 1.02 | 0.33 | 24% |
| Cursor | 67.4% | 39.2% | 49.6% | 1.38 | 0.72 | 0.35 | 33% |
| Greptile | 51.5% | 47.3% | 49.3% | 2.10 | 0.88 | 0.82 | 49% |
| CodeRabbit | 39.0% | 43.2% | 41.0% | 2.33 | 0.80 | 1.25 | 61% |
| Copilot | 26.7% | 37.8% | 31.3% | 3.27 | 0.70 | 1.93 | 73% |
| *TraceHound diff-v1 (055)* | 25.6% | 43.2% | 32.2% | 3.58 | 0.80 | 2.33 | 74% |
| *TraceHound verify-ctx-v1 (058)* | 38.9% | 28.4% | 32.8% | 1.57 | 0.53 | 0.82 | 61% |
| *TraceHound pool-raw (059)* | 24.5% | **51.4%** | 33.2% | 6.08 | 0.95 | 2.93 | 75% |
| *TraceHound pool-verify-v1 (059, adopted)* | 36.1% | 35.1% | **35.6%** | 2.08 | 0.65 | 1.15 | 64% |

"Share wrong" = FP / (TP + FP). Per-PR rates divide by 40.

## Each tool's profile

- **Augment: the target.** It finds the most real defects (1.02 per PR) and makes the fewest wrong comments (0.33 per PR). It beats us on both axes, so it isn't trading one for the other. Best on keycloak (F 73.3%, P 91.7%).
- **Cursor: conservative.** Few comments with high precision, but it misses 61% of defects. Best on sentry (P 81.8%).
- **Greptile: balanced.** Strong on grafana and discourse (F 59–60%), weaker on sentry and keycloak (40%).
- **CodeRabbit: recall, at a cost.** Recall equals ours (43.2%), with more noise than Greptile. This is the realistic ceiling for us before the freeze.
- **Copilot: noisy.** It has the most comments of any tool, and nearly 3 of 4 are wrong. This is our closest peer by profile.

## Per repo, F (dev)

| Repo (n) | Augment | Cursor | Greptile | CodeRabbit | Copilot | diff-v1 | ctx-v1 | verify-ctx-v1 | agree |
|---|---|---|---|---|---|---|---|---|---|
| sentry (21) | 62.9 | 56.3 | 40.0 | 41.9 | 23.1 | 28.6 | 28.6 | 27.0 | 25.8 |
| grafana (16) | 59.3 | 51.9 | 59.3 | 48.0 | 41.4 | 34.5 | 27.3 | 20.0 | 11.8 |
| keycloak (18) | 73.3 | 48.0 | 40.0 | 39.0 | 34.0 | 25.7 | 40.0 | 35.9 | 50.0 |
| discourse (19) | 61.1 | 42.4 | 60.0 | 38.3 | 31.4 | **45.5** | 30.3 | 43.8 | 28.6 |

pool-verify-v1 (059) per repo: sentry 27.3, grafana 34.8, keycloak 37.2, discourse 44.4.

Per-repo n is 16–21, so one comment moves F by several points. Treat these as hints, never as claims.

## Where we stand, honestly

- **We beat:** Copilot on F with diff-v1 (32.2 vs 31.3), which is within noise.
- **We tie:** CodeRabbit on recall (32 TPs each).
- **Closest to the field:** discourse, where diff-v1 (45.5) is above CodeRabbit, Copilot and Cursor. A per-repo hint only.
- **After 058, our wrong comments per PR (0.82) equal Greptile's.** The noise problem is mostly solved. The gap to Greptile is now real defects found: 0.53 vs 0.88 per PR. **The bottleneck has moved from precision to recall**, which is why 059 widens the candidate pool.
- **059:** the raw pool (diff-v1 plus ctx-only) reaches **51.4% recall, above Greptile (47.3), CodeRabbit (43.2), Cursor and Copilot**; only Augment (55.4) is higher. The defects are in our candidates. The filter is what loses them: pool-verify-v1 keeps 26 of 38 TPs and lands at F 35.6. That's above Copilot and below CodeRabbit (41.0).
- **Far behind:** Augment and Cursor on precision, and Augment on everything.

## Published claims (they disagree)

On the same 50 PRs:
- Greptile reports an 82% catch rate for itself and 44% for CodeRabbit.
- Tenki reports recall of 36% and 29%.
- Augment reports F of 45% (Greptile) and 39% (CodeRabbit).

Our judge, on all 50 PRs, gives a higher F than Augment's published figure for every tool (+1.0 to +10.2 points), with the same order.
