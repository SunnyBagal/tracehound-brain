# TraceHound submission: the numbers sheet (single source of truth)

Written by chat, 27 Oct 2026. **Every lane copies numbers and claims from this file only.** If a number isn't here, it isn't published. Sources are TraceHound `docs/bench/*` at main `36de717` and the brain. `[[TBD: …]]` marks the only open slots.

## 0. Wording rules (hard)
1. Never "beats", "best", "state of the art", "outperforms".
2. "ahead of / behind X" only where the 069b 95% interval excludes 0 (table §2). Everything else is "statistically tied with X".
3. "3rd of 6" only with "point estimate" next to it.
4. **No competitor names or logos in the video or the Devpost text** (R14). Use "five commercial reviewers" / "the leading commercial reviewer". Names are fine in the repo (README, results.md), where the replication material is.
5. The product is **pool-verify-v1 on Nemotron 3.5 Super (`nvidia/Nemotron-3.5-Super-VL`) at tag `review-freeze-2` (`0aeef06`)**, via Nebius Token Factory. Every product claim describes that config.
6. Dev numbers: "on our 40-PR dev set" (tuned on). 3.5 on dev is "2 dev runs".
7. ReviewBench: "ReviewBench test runs (25 PRs, one round, ReviewBench's own judge); not a leaderboard submission". Never imply a leaderboard rank.
8. Ultra: "report only, free NVIDIA API, not Token Factory, not the product".
9. Losses are stated as losses (§6).
10. "No network" wording for the analyzer: "no model calls; network only for a credential-free git fetch of the base commit".

## 1. Headline: blind Cal.com test (10 PRs, n = 23 golden comments), decision 069
Pre-registered before the run; chosen on dev before this look; no tuning after; one run.
| reviewer | P | R | F (judge v1, scorer of record) | F (judge x1) |
|---|---|---|---|---|
| **TraceHound v2 (pool-verify-v1, 3.5)** | 63.2% (12/19) | 52.2% (12/23) | **57.1%** | 58.5% |
| TraceHound v1 (3 Super, 062, run 1) | 56.0% | 60.9% | 58.3% | 58.3% |
| Augment | 83.3% | 65.2% | 73.2% | 73.2% |
| Cursor | 81.3% | 56.5% | 66.7% | 66.7% |
| Greptile | 58.8% | 43.5% | 50.0% | 55.0% |
| Copilot | 35.7% | 65.2% | 46.2% | 50.8% |
| CodeRabbit | 34.6% | 39.1% | 36.7% | 37.5% |
Allowed: "On 10 blind Cal.com PRs, TraceHound is statistically tied with four of five commercial reviewers and ahead of one (CodeRabbit; only 10 PRs)."

## 2. All 50 PRs (dev 40 + Cal.com 10, n = 97), 069 + 069b
| reviewer | F v1 | F x1 |
|---|---|---|
| Augment | 66.3% | 65.9% |
| Cursor | 53.8% | 58.8% |
| **TraceHound v2** (dev 7r + Cal.com 4) | **52.2%** | **55.4%** |
| Greptile | 49.5% | 55.1% |
| CodeRabbit | 40.0% | 45.0% |
| Copilot | 35.2% | 42.0% |
069b paired bootstrap (10,000, seed 69), both judges agree:
- **behind Augment**; **statistically tied with Cursor and Greptile**; **ahead of Copilot and CodeRabbit**.
- v2 vs v1 (3 Super): ahead under v1 (+11.0 [+1.8, +20.1]), tied under x1.
Allowed: "Across all 50 PRs: behind the leading reviewer, statistically tied with the next two, ahead of the last two (3rd of 6 as a point estimate)." In the repo the names may be given.

## 3. Dev set (40 PRs, n = 74; tuned on)
- pool-verify-v1: 3 Super 5-run mean F 42.3 (P 40.5); **3.5 2-run mean F 50.4 (P 54.8)**; x1 51.3 → 55.1.
- pool-raw (unverified pool): 3 Super 38.2; 3.5 53.9.
- Commercial tools' dev F (v1): 64.1 / 49.6 / 49.3 / 41.0 / 31.3.
- Run-to-run spread at temperature 0 (3 Super pool-verify, F): 35.6–48.0.
- Cost per PR (pipeline, pre-VAT): 3 Super median $0.031; **3.5 median $0.063, mean $0.074**. 3.5 fails valid JSON on ~7 of 40 PRs per run (a chunk lost).

## 4. ReviewBench test runs (P008, image `9b9a5127`, 25 PRs, one round, ReviewBench's judge)
| # | config | grounded F1 | G P | G R | augmented F1 | A P | findings |
|---|---|---|---|---|---|---|---|
| 331 | **pool-raw-x2 · 3 Super** (pre-registered winner) | **21.3** | 76.3 | 12.4 | 30.6 | 50.6 | 222 |
| 333 | pool-raw-missed · 3 Super | 20.4 | 64.5 | 12.1 | 28.9 | 43.9 | 194 |
| 328 | pool-raw · 3 Super | 16.0 | 70.6 | 9.0 | 23.0 | 52.5 | 124 |
| 330 | pool-verify-v1 · 3 Super | 13.9 | 79.5 | 7.6 | 22.3 | 67.2 | 99 |
| 329 | pool-raw · 3.5 | 13.5 | 56.3 | 7.7 | 19.0 | 44.0 | 86 |
| 332 | pool-verify-v1 · 3.5 (= the product) | 12.2 | 69.2 | 6.7 | 17.6 | 57.2 | 60 |
- No final run / no leaderboard entry: the final (219 × 3) would cost ~$107 with VAT (measured from #331: $3.45 per 25-PR round) against a $43.52 balance (P008 amendment 8).
- Six configs on the same 25 PRs: selection bias, stated.
- **Finding (honest):** 3.5 beat 3 Super on our dev set but lost on ReviewBench in both pipelines (12.2 vs 13.9; 13.5 vs 16.0). ReviewBench's grounded metric rewards volume (findings), which the product's verifier cuts.
- Leaderboard context only in the repo, never as our rank.

## 5. Ultra (071, report only, free NVIDIA API)
- Run 10, judge v1: pool-verify-v1 F 42.3 (P 34.8 / R 54.1) vs 3 Super 42.3, 3.5 50.4; pool-raw F 42.1 vs 3.5 53.9 → **worse than 3.5 on pool-raw (−11.8 [−19.6, −4.0])**; no other interval-backed difference. x1: 54.6 / 51.8.
- Run 11: [[TBD: run 11 + 2-run mean, from 071b]].
- Allowed: "The 550B Nemotron 3 Ultra did not review better than 3.5 Super on our pipeline (report only)."

## 6. Losses (state them)
- Graph did not help the repair agent (050). Context-aware verifier (058), vote across runs (064), evidence verifier (065), recall-v3 (070: both arms below P009's precision floor), Ultra (071).
- v3 measured partly hampered (truncation, per-PR cap); 2 v3 defects found by Bugbot, documented not fixed.
- No ReviewBench final (money).

## 7. Live demo
- `SunnyBagal/tracehound-demo`, Action pinned `@0aeef06` with the explicit 3.5 model. **Demo PR #2 under 3.5 posted no findings (24 Oct)**; the older posted reviews were Nano generators and must not be shown as the current config. **Demo PR #6 is never merged.**
- [[TBD: seeded-PR run on the current config (job seed-01): PRs opened, seeded lines caught / total, links]].

## 8. Cost and runtime
- Product: ~$0.06–0.07 per PR on 3.5 (pre-VAT), ~2 min wall median per PR on dev.
- Whole project Nebius spend (30 days to 24 Oct): $57.05 + VAT.

## 9. What's next (honest roadmap, from levers v2 + the 27 Oct review)
- Recover chunks lost to invalid JSON (JSON repair / schema-constrained output; retry-with-reasoning-off mostly failed in 070).
- Per-PR comment cap (product noise) — [[TBD: levers v2 replay result]].
- The verifier doesn't pay on 3.5 (pool-raw ≥ pool-verify on dev and ReviewBench) — [[TBD: levers v2 table]].
- Cheaper experiments: run-to-run spread exceeds most effects; replay on saved records before paying for runs.
