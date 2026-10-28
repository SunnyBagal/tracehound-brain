# TraceHound submission: the numbers sheet (single source of truth)

Written by chat, 27 Oct 2026. **Every lane copies numbers and claims from this file only.** If a number isn't here, it isn't published. Sources are TraceHound `docs/bench/*` at main `36de717` and the brain. No open slots (§7 filled 28 Oct 19:20).

## 0. Wording rules (hard)
1. Never "beats", "best", "state of the art", "outperforms".
2. "ahead of / behind X" only where the 069b 95% interval excludes 0 (table §2). Everything else is "statistically tied with X".
3. "3rd of 6" only with "point estimate" next to it.
4. **No competitor names or logos in the video or the Devpost text** (R14). Use "five commercial reviewers" / "the leading commercial reviewer". Names are fine in the repo (README, results.md), where the replication material is. **Don't name Cursor in the Devpost text or video even as a coding tool used** (it's also a benchmarked reviewer); Bugbot's catches are credited to "an automated review".
5. The product is **pool-verify-v1 on Nemotron 3.5 Super (`nvidia/Nemotron-3.5-Super-VL`) at tag `review-freeze-2` (`0aeef06`)**, via Nebius Token Factory. Every product claim describes that config. **The Action to pin is `review-freeze-2.1` (`86fd5d7`, decision 072)**: `review-freeze-2` plus one commit that changes only the Action's install step (pnpm via npm, so a reviewed repo's `packageManager` is never read); `git diff 0aeef06 86fd5d7` = `action.yml` only, review code byte-identical. Wording: "the review pipeline at `review-freeze-2`; the Action at `review-freeze-2.1` fixes installation only".
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
- **2 runs (10, 11), judge v1:** pool-verify-v1 mean P 39.0 / R 59.5 / **F 47.1** (3 Super 42.3, 3.5 50.4); pool-raw mean F 43.6 (3.5 53.9). x1: 54.9 / 51.4. **Only interval excluding 0: pool-raw v1, Ultra − 3.5 = −10.2 [−17.7, −3.2].** Everything else: no interval-backed difference. Run 11: 57 HTTP 429s of 361 NVIDIA requests. Judge spend 071 total $0.19.
- Allowed: "The 550B Nemotron 3 Ultra did not review better than 3.5 Super on our pipeline (report only)."

## 6. Losses (state them)
- Graph did not help the repair agent (050). Context-aware verifier (058), vote across runs (064), evidence verifier (065), recall-v3 (070: both arms below P009's precision floor), Ultra (071).
- v3 measured partly hampered (truncation, per-PR cap); 2 v3 defects found by Bugbot, documented not fixed.
- No ReviewBench final (money).

## 7. Live demo
- `SunnyBagal/tracehound-demo`, Action pinned `@0aeef06` (to move to `86fd5d7`, review-freeze-2.1) with the explicit 3.5 model. **Demo PR #2 under 3.5 posted no findings (24 Oct)**; the older posted reviews were Nano generators and must not be shown as the current config. **Demo PR #6 is never merged.**
- **Seeded test (seed-01, pre-registered 19 Oct: seeds, key and scorer committed at `a4a6c83`, hashes published before any PR; scored 28 Oct by the committed scorer, automatic rule; `tracehound-seeds` `88a43c6`, RESULTS.md):** 30 PRs on two mirrors of the owner's apps (Recall, Linea): **22 seeded defects + 8 clean controls**. pool-verify-v1 on 3.5; Recall ran the Action at `0aeef06`, Linea at `86fd5d7` (review-freeze-2.1, the install fix this test found).
  | class | caught | Recall | Linea |
  |---|---|---|---|
  | in-diff (D) | **5/6** | 3/3 | 2/3 |
  | cross-file, typed | 2/6 | 2/3 | 0/3 |
  | cross-file, keyed | 2/6 | 1/3 | 1/3 |
  | cross-file, semantic | 0/4 | 0/2 | 0/2 |
  | **all seeded** | **9/22** | 6/11 | 3/11 |
  | controls with a comment | **0/8** | 0/4 | 0/4 |
  - **3 of the 4 cross-file catches came from the deterministic contract check** (no model; the review's "files you didn't edit that depend on this change" section); 1 came from the model. Say so wherever a cross-file catch is shown.
  - 17 of 30 PRs got no review (green run, nothing to post), including all 8 controls. 21 comments on 30 PRs (0.70 per PR). 0 partial reviews; every final run green.
  - Manual column: not used (headline = automatic rule only). Two near-misses (the review names the right file and line but not the key symbol) are described in RESULTS.md and not counted.
  - Not run: CodeRabbit, Greptile, Bugbot (so no comparison with any other reviewer). Model 3.5, not the 3 Super the 19 Oct plan assumed.
  - Deviations (disclose with the result): Linea opened after the Action fix, same order; open-PR script copy with 4 disclosed changes; linea#1 base re-set once (no content change) and re-triggered twice; a third-party review app auto-commented on the first 3 PRs (not scored; removed); first-PR gate widened to 3 PRs.
  - **Allowed sentence:** "On 30 pre-registered seeded PRs, TraceHound caught 9 of 22 seeded defects and stayed silent on all 8 clean controls. It caught 5 of 6 bugs inside the diff but only 4 of 16 that break files the PR didn't touch, and 3 of those 4 came from the deterministic contract check, not the model."
  - Found by this test: the Action couldn't install on repos pinning another pnpm (fixed as review-freeze-2.1, decision 072).
  - Video: model catch `https://github.com/SunnyBagal/recall-seeded/pull/4#discussion_r4393814033` (check-then-act race); contract-check catches `https://github.com/SunnyBagal/recall-seeded/pull/7#pullrequestreview-5673943375` (renamed route still used in an unedited file) and `https://github.com/SunnyBagal/recall-seeded/pull/6#pullrequestreview-5673929237` (type error at shared.tsx:81); miss `https://github.com/SunnyBagal/recall-seeded/pull/12` (callers now get an exception instead of null).
  - **Requires `SunnyBagal/tracehound-seeds` to be public** before the README links RESULTS.md.

## 8. Cost and runtime
- Product: ~$0.06–0.07 per PR on 3.5 (pre-VAT), ~2 min wall median per PR on dev.
- Whole project Nebius spend (30 days to 24 Oct): $57.05 + VAT.

## 9. What's next (honest roadmap, from levers v2 + the 27 Oct review)
- Recover chunks lost to invalid JSON (JSON repair / schema-constrained output; retry-with-reasoning-off mostly failed in 070).
- Per-PR comment cap: replayed on saved dev runs ($0, levers v2, PR #64): **no cap on the shipped (verified) pipeline improves F with an interval excluding 0**; small gains appear only on the unverified pool (k = 4–5). Hypothesis, not adopted.
- The verifier: on dev, verified beats raw with an excluding interval in 2 of 9 runs (3 Super 2r, Ultra 11); on 3.5 neither run shows a difference (−1.6, −5.4, intervals include 0). On ReviewBench raw scores higher in both models (volume metric). Open question, not a finding.
- A cheaper 2-of-3 vote across runs: no gain established (levers v2).
- Where the misses are: both 3.5 runs miss 35 of 74 Medium+ golden comments (v1); none of the 7 Critical ones is missed by both; the most missed repo is sentry (14 of 21 Medium+).
- Cheaper experiments: run-to-run spread exceeds most effects; replay on saved records before paying for runs.
