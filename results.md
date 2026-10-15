# Results (dev set only)

Dev set: sentry, grafana, keycloak, discourse; 40 PRs; n = 74 golden comments of Medium or higher. Judge `bench-judge-v1` (Nemotron Super, temperature 0, one judge). Every row is one run. Differences of a few points are noise.

Cal.com (test set) numbers stay out of this public vault until the final run is published.

| Reviewer | Decision | P | R | F | Note |
|---|---|---|---|---|---|
| review-diff-v1, Super | [055](decisions/D055-diff-only-baseline.md) | 25.6% (32/125) | 43.2% (32/74) | 32.2% | baseline |
| review-diff-v1, Nano | [055](decisions/D055-diff-only-baseline.md) | 25.4% | 21.6% | 23.4% | |
| verify-v1 (threshold 1, pre-set rule) | [056](decisions/D056-verify-v1.md) | = diff-v1 | = diff-v1 | = diff-v1 | keeps everything |
| verify-v1 (threshold 5, after the fact) | [056](decisions/D056-verify-v1.md) | 44.4% | 21.6% | | not a valid pick |
| review-ctx-v1, Super | [057](decisions/D057-repo-context.md) | 36.2% (21/58) | 28.4% (21/74) | 31.8% | tie |
| verify-ctx-v1, Super | [058](decisions/D058-verify-ctx.md) | 38.9% (21/54) | 28.4% (21/74) | 32.8% | loss by pre-set rule (F ≥ 35.0%) |
| agree-diff-ctx, Super (report only) | [058](decisions/D058-verify-ctx.md) | 57.7% (15/26) | 20.3% (15/74) | 30.0% | highest precision so far |

## Recorded tools on the same dev set and judge

| Tool | P | R | F |
|---|---|---|---|
| Augment | 75.9% | 55.4% | 64.1% |
| Cursor | 67.4% | 39.2% | 49.6% |
| Greptile | 51.5% | 47.3% | 49.3% |
| CodeRabbit | 39.0% | 43.2% | 41.0% |
| Copilot | 26.7% | 37.8% | 31.3% |

The golden set was extended by Augment using these tools' own comments, which favours them (see [D054](decisions/D054-review-benchmark-scorer.md)).
