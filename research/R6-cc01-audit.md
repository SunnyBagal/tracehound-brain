# R6 · Independent audit of the benchmark claims (cc-01, Claude cloud)

**Source:** Claude cloud session, 18 Oct; draft PR **#41** (`docs/bench/audit-cc01.md`).
**Chat check (18 Oct):** the wording problems are confirmed in `results.md:16-20` and `:110-113`. The tools' Cal.com counts were in `scorer-validation.md` from 054, so "pre-registered before any Cal.com result existed" is false as worded. The correct wording is "before any TraceHound Cal.com result existed".

**Fixes to make (judges' README / results-page job):**
1. "pre-registered before any **TraceHound** Cal.com result existed".
2. "the headline run was run 1 of three, all from `e7309a5`, whose frozen paths are identical to the tag `review-freeze-1`" (instead of "ran once from frozen code").
3. "The judge is **the same model** (Nemotron Super) as the reviewer and verifier". Drop "(060: the gap is small)".
4. Replace "changes nothing" with what x1 actually shows: it reverses (a) in runs 1 and 3, and drops run 3 to 5th.
5. Add the all-50 table (v1 and x1), the dev-run-2r caveat, and the audit's interval statement: "every gap except CodeRabbit's is within noise".
6. Add per-PR Cal.com counts (counts only) to the repo, so a real PR-level bootstrap is possible. That's a relay job.

**Chat's reading:** the "3rd of 6" headline can't stand as a ranking. The honest public line is "on 10 blind PRs, statistically tied with Greptile and Cursor and ahead of CodeRabbit; 4th of 6 across all 50".

---

I've read the README section and five of the bench docs; now reading decisions 054–063, then I'll recompute every P/R/F with a script.

Recomputed all 486 "x% (a/b)" fractions and 219 table rows: no arithmetic mismatches so far. Next: cross-checks (sums, ranks, spreads, prose numbers), then the 50-PR table and bounds.

All cross-checks have passed, and the 50-PR table is built. Next I'll compute uncertainty bounds, since Cal.com's per-PR counts aren't available in the repo.

Numbers all reconcile; the findings are framing and statistical. Writing the audit file now.

I found three issues when cross-checking my doc against the sources: the judge-disagreement rate needs to be split by kind, a tool-range comparison was wrong (Cursor's Cal.com F is also well above its dev range), and an appendix line got garbled. I'll fix all three now.

I wrote the audit in `docs/bench/audit-cc01.md` (one commit) and opened it as draft PR [SunnyBagal/TraceHound#41](https://github.com/SunnyBagal/TraceHound/pull/41). It's not merged. Nothing was fetched and no model was called; the audit uses only counts already in the repo. All the numbers in the docs are correct, but the "3rd of six" headline overstates a ranking that a 10-PR test set can't separate from ties.

**Mismatches: none in any number.** I rechecked 486 `x% (a/b)` figures, 219 table rows, every sum, the spread tables, kappa (0.809 and 0.395), tau-b (0.891), augmented precision, ranks and spend. All five problems are in the wording of `results.md`:
- **`results.md:19-20`** says the plan was registered "before any Cal.com result existed". The five tools' Cal.com scores were already in the repo from decision 054 (`scorer-validation.md:62-70`).
- **`results.md:20`** says the test "ran once from frozen code, tag `review-freeze-1` → `6b4276e`". Cal.com ran three times, all from `e7309a5`.
- **`results.md:112`** says the judge is the "same model family" as the reviewer. It is the same model, Nemotron Super, which also does the filtering step.
- **`results.md:112`** says "(060: the gap is small)". Decision 060 said "draw no conclusion beyond them", and pool-verify-v1's precision gap is −11.6 points against −6.6 for the tools.
- **`results.md:18-19`** says the second judge "changes nothing". Under that judge, prediction (a) fails in Cal.com runs 1 and 3.

**Unsupported or overstated claims:**
- **"ranked 3rd of six … ahead of Greptile, Copilot and CodeRabbit"**: right as a single-run result, not shown as a ranking. Two judge decisions would tie TraceHound with Greptile, and two the other way would tie it with Cursor.
- **"treat single-run ranks as approximate"**: understated. Apart from CodeRabbit, the gaps to the tools can't be told apart from noise.
- **"On the dev set it ranked 5th"**: true for dev run 1; dev run 2r was 4th, which the README doesn't say.

**All 50 PRs (dev run 1 + Cal.com run 1, n = 97):**

| reviewer | F, bench-judge-v1 (scorer of record) | F, bench-judge-x1 |
|---|---|---|
| Augment | 66.3% | 65.9% |
| Cursor | 53.8% | 58.8% |
| Greptile | 49.5% | 55.1% |
| **TraceHound (4th)** | **41.2%** | **48.1%** |
| CodeRabbit | 40.0% | 45.0% |
| Copilot | 35.2% | 42.0% |

If dev run 2r replaces run 1, TraceHound moves to 3rd under v1 (50.5%) but stays 4th under x1.

**Confidence intervals:** a bootstrap over PRs can't be done, because the repo has no per-PR counts for Cal.com. The best substitute is a resampling of the individual comment results, widened by a factor of 1.27 to allow for comments clustering within PRs (estimated from the dev per-PR data). On that basis the 95% interval includes zero for every tool gap except CodeRabbit's. Averaged over the three Cal.com runs, TraceHound scores 54.2% under the second judge, which puts it 4th, behind Greptile's 55.0%.

**Top 3 threats:**
1. No uncertainty is reported anywhere for a test set of 10 PRs and 23 golden comments. The docs say only that "n is small".
2. Nemotron Super writes the review, filters it and scores it. The docs disclose this but soften it, as in the two `results.md:112` problems above.
3. The headline uses TraceHound's best repo: its Cal.com F is 13.9 points above its best dev repo. Across all 50 PRs it is 4th, and no combined table is shown.

The audit file also covers seven lower-ranked threats with where each is disclosed, and four suggested fixes. Since this is a docs-only draft I haven't subscribed to the PR; I can watch it for review comments if you want.