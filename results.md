# Results

Every measured result, newest last. One run per row unless stated. Differences of a few points are noise.

## Reviewer, dev set (40 PRs; n = 74 golden comments ≥ Medium; judge `bench-judge-v1`)

| Reviewer | Decision | comments | TP | FP | dup | low | FN | P | R | F | Verdict |
|---|---|---|---|---|---|---|---|---|---|---|---|
| review-diff-v1, Super | [055](decisions/D055-diff-only-baseline.md) | 143 | 32 | 93 | 8 | 10 | 42 | 25.6% | 43.2% | 32.2% | baseline; what the Action runs today |
| review-diff-v1, Nano | [055](decisions/D055-diff-only-baseline.md) | 66 | 16 | 47 | 2 | 1 | 58 | 25.4% | 21.6% | 23.4% | Nano too weak |
| verify-v1, threshold 1 (pre-set rule) | [056](decisions/D056-verify-v1.md) | 143 | 32 | 93 | | | 42 | 25.6% | 43.2% | 32.2% | loss: keeps everything |
| verify-v1, thresholds 2–5 (after the fact) | [056](decisions/D056-verify-v1.md) | | | | | | | → 44.4% | → 21.6% | 32.1 / 31.2 / 29.2 / 29.1% | F falls as P rises |
| review-ctx-v1, Super | [057](decisions/D057-repo-context.md) | 133 | 21 | 37 | 4 | 71 | 53 | 36.2% | 28.4% | 31.8% | tie |
| verify-ctx-v1, Super | [058](decisions/D058-verify-ctx.md) | 63 | 21 | 33 | 3 | 6 | 53 | 38.9% | 28.4% | 32.8% | loss by bar (35.0%) |
| agree-diff-ctx (report only) | [058](decisions/D058-verify-ctx.md) | 33 | 15 | 11 | 2 | 5 | 59 | 57.7% | 20.3% | 30.0% | highest P |
| pool-raw (report only) | [059](decisions/D059-pool-verify.md) | 243 | 38 | 117 | 10 | 78 | 36 | 24.5% | 51.4% | 33.2% | highest R of ours |
| **pool-verify-v1, Super** | [059](decisions/D059-pool-verify.md) | 83 | 26 | 46 | 4 | 7 | 48 | 36.1% | 35.1% | **35.6%** | **adopted** (bar 35.0; one TP flip → 34.2) |

### Under a second judge (060, bench-judge-x1 = Qwen3-235B, report only)

| Run | F under v1 | F under x1 | Rank v1 → x1 (of 11, incl. the 5 tools) |
|---|---|---|---|
| Augment | 64.1 | 63.6 | 1 → 1 |
| Cursor | 49.6 | 56.1 | 2 → 2 |
| Greptile | 49.3 | 55.1 | 3 → 3 |
| CodeRabbit | 41.0 | 47.4 | 4 → 4 |
| **pool-verify-v1** | **35.6** | **44.6** | **5 → 5** |
| pool-raw | 33.2 | 40.2 | 6 → 6 |
| verify-ctx-v1 | 32.8 | 38.1 | 7 → 8 |
| review-diff-v1 | 32.2 | 35.7 | 8 → 9 |
| review-ctx-v1 | 31.8 | 34.8 | 9 → 10 |
| Copilot | 31.3 | 38.9 | 10 → 7 |
| agree-diff-ctx | 30.0 | 28.3 | 11 → 11 |

Agreement: 78.6% same label, kappa 0.809, tau-b 0.891. Augmented precision (validity-v1, Super): pool-verify-v1 88.9%, Augment 92.6%, Cursor 90.7%, Greptile 88.2%, CodeRabbit 84.1%, Copilot 51.4%. This is weak evidence (validity judges' kappa 0.395; Super judging Super-verified comments) ([D060](decisions/D060-scorer-checks.md)).

### Per repo, F (n: sentry 21, grafana 16, keycloak 18, discourse 19)

| Reviewer | sentry | grafana | keycloak | discourse |
|---|---|---|---|---|
| review-diff-v1, Super | 28.6 | 34.5 | 25.7 | 45.5 |
| review-diff-v1, Nano | 9.5 | 24.0 | 36.8 | 25.0 |
| review-ctx-v1 | 28.6 | 27.3 | 40.0 | 30.3 |
| verify-ctx-v1 | 27.0 | 20.0 | 35.9 | 43.8 |
| agree-diff-ctx | 25.8 | 11.8 | 50.0 | 28.6 |
| pool-raw | 30.3 | 35.3 | 27.8 | 51.4 |
| pool-verify-v1 | 27.3 | 34.8 | 37.2 | 44.4 |

Commercial tools on the same set: [competitors](benchmark/competitors.md).

### Verifier behaviour against the 055 judge outcomes

| Verifier | TPs kept | FPs kept | Separates? |
|---|---|---|---|
| verify-v1 (confidence; 6 of 32 TPs got confidence 1) | — | — | barely |
| verify-ctx-v1 (keep/drop with context) | 22 of 32 (69%) | 31 of 93 (33%) | yes |

### Context reached the model?

- **057:** references were shown in 12 of 48 chunks (147 of 1,088 selected). Whole files were dropped in 33 chunks.
- **058:** references were shown in 97 of 143 findings (median 6, max 15). Median context 12,329 chars per finding.

## Reviewer cost and speed (our ledger; prices are third-party estimates)

| Run | Total | Per PR | Wall per PR (median / max) |
|---|---|---|---|
| review-diff-v1, Super (40 PRs) | $0.4988 | ~$0.012 | 31.5 s / 213.9 s |
| review-ctx-v1, Super | $0.8313 | ~$0.021 | 47.7 s / 330.9 s |
| verify-v1 (143 findings) | $0.19 | ~$0.005 | — |
| verify-ctx-v1 (143 findings) | $0.3225 | ~$0.008 | 6.5 s / 31.0 s |
| verify-ctx-v1 on 100 ctx-only findings (059) | $0.18675 | — | — |
| **Full pool pipeline** (diff + ctx + verifier, 059) | — | median $0.0317, max $0.2308 | 100 s / 581 s (sequential) |
| Judge, one 40-PR scoring | ~$0.026 | — | — |

## Earlier phases

- **Held-out repair evaluation (050),** 160 runs. Resolved, graph on vs off: Nano 11 vs 13, Super 28 vs 26 (n = 40 each). Verified at stop: Nano 11 vs 19, Super 30 vs 31. 0 graph tool calls in 80 graph-on runs. [phase 2](history/phase-2-repair-eval.md)
- **Reproduce stage (048), dev:** Super reproduced 3 of 4 seeded claims and 1 of 4 controls; Nano 0 of 4. The repair agent on Nano resolved 1 of 3 reproduced tasks.
- **Study S1, BullMQ on 10 unseen repos:** pairing rate 2.1% (4 of 179 producers), one false edge. Both kill criteria met. [phase 1](history/phase-1-graph.md)
- **Naming audit (1 Oct):** Recall had 5 of 7 summaries wrong, CEX 2 of 7.
- **Agent toy gate:** agent-v2, graph off, 2 of 5 (needed 4 of 5). v4 diagnostics: reasoning on 5/5, off 4/5.

## Blind test: Cal.com (decision 062, pre-registered in 061; 10 PRs, n = 23; frozen at `review-freeze-1`)

| Reviewer | F run 1, v1 (scorer of record) | F run 1, x1 | Rank run 1/2/3, v1 | Rank run 1/2/3, x1 |
|---|---|---|---|---|
| Augment | 73.2 | 73.2 | 1 | 1 |
| Cursor | 66.7 | 66.7 | 2 | 2 |
| **pool-verify-v1** | **58.3** (P 56.0, R 60.9) | 58.3 | **3 / 3 / 3** (58.3, 51.3, 52.8) | **3 / 3 / 5** (58.3, 57.1, 47.3) |
| Greptile | 50.0 | 55.0 | — | — |
| Copilot | 46.2 | 50.8 | — | — |
| CodeRabbit | 36.7 | 37.5 | — | — |
| review-diff-v1 (ours) | 53.8 | 60.0 | | |
| pool-raw (ours) | 51.6 | 56.1 | | |

- The tools were judged once on recorded reviews; our rows were run 3 times.
- Predictions: (a) above review-diff-v1, right under v1 but reversed under x1; (b) 5th, **wrong** (3rd); (c) outside 30.6–40.6%, right.
- **Dev spread:** pool-verify-v1 v1 F 35.6 (059) → 48.0 (2r). At temperature 0, run-to-run noise is larger than every margin the reviewer was chosen on. Dev run 3 was not done (cap).
