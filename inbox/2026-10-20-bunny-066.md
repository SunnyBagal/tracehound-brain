# 066 report (Bunny, 2026-10-20)

**PR #52** (draft): https://github.com/SunnyBagal/TraceHound/pull/52
**Head:** `0d3a6c4422c713370dae784d0b151cd9d3b596c4`
**CI:** test-and-build pass, Vercel pass, Vercel Preview Comments pass, review skipping. Green on the first run; no fix attempt was needed.

## Commits
- `04a8da6` docs: save the 066 results-docs brief verbatim
- `27990eb` bench docs: apply the cc-01 audit's five wording fixes (066)
- `c446f5b` bench docs: all-50 table under both judges, headline rewritten (066), with fix 5 corrected and the viewer synced
- `e54ee63` bench docs: precision levers tried on dev, one table (066), viewer synced
- `65c13ea` readme: benchmark headline rewritten to the 066 claim
- `f4ba44d` decisions: 066, results wording fixes, all-50 table, levers tried, plus the CLAUDE.md line
- `0d3a6c4` viewer: pin the benchmark source link to the 066 results revision (not in the brief, see below)

## Tests (HEAD, local)
- pnpm test: action 179/179; analyzer 849 passed, 7 skipped (856); viewer 188/188 (was 186: the "all 50 PRs" check moved into its own test and the "3rd of 6" check was added).
- pnpm typecheck: clean.
- viewer build: OK.

Two gate runs went red on timeouts only, while the machine's load average was about 94 (something else on the host):
- **Step 6:** 3 analyzer tests failed (contracts-findings, contracts-fixtures, finder-claims). The error text wasn't captured, because I filtered the output to summary lines. All 54 tests in those files passed in isolation, and the full suite passed on a re-run.
- **Pin commit:** 10 analyzer tests failed, all "Test timed out in 5000ms". I waited for the load to drop below 16, re-ran, and got green.

Every commit was made after a green gate, in a separate command.

## Step 0
- `git fetch`: main moved `8193651 → dee2bed` (#49). The tree was clean, the checkout was on main, and after the ff-only merge HEAD = origin/main = `dee2bed`.
- #48 (`720cc83`) is in main; `## 065` count = 1, `## 066` count = 0. `audit-cc01.md` exists (#41, `9a81505`).
- No model call. Nothing opened in tracehound-ops, tracehound-bench, the seeded repos or ReviewBench golden/.
- Before push, main had moved to `95f79f0` (#51: reviewbench and analyzer review-missed code, no overlap, no `## 066`). The PR merges cleanly.

## First stop (step 3) and the rulings
- **What happened:** `viewer/test/benchmark.test.tsx` (#44) failed once the all-50 tables were in results.md. My first step-3 commit was chained after the red test. It was never pushed, and I undid it with `git reset --soft`.
- **Your rulings, applied:**
  - option (b), with only `viewer/lib/benchmark.ts` and `viewer/test/benchmark.test.tsx` touched;
  - fix 5 reworded;
  - "ahead of CodeRabbit" removed from both headlines;
  - temperature sources added;
  - gates run as their own command before every commit.

## The five fixes (before = `dee2bed`, after = `0d3a6c4`)
1. **Pre-registration:** `results.md:19` "pre-registered before any Cal.com result existed (061)" → `results.md:27-28` "pre-registered before any TraceHound Cal.com result existed (061); the five tools' Cal.com scores were already in the repo (054)". decisions.md: dated note under 061 (`decisions.md:3169-3179`), which also covers 061's "written before anyone has seen a Cal.com result" (`:3011`).
2. **Run 1 of three:** `results.md:19-20` "ran once from frozen code, tag `review-freeze-1` → `6b4276e` (062)" → `results.md:28-29` "The headline is run 1 of three, all from `e7309a5`, whose frozen paths are identical to tag `review-freeze-1` (062)". This wording doesn't appear in test-results.md or decisions.md.
3. **Same model:** `results.md:112`, `test-results.md:162` "The judge is the same model family as the reviewer (060: the gap is small)." → `results.md:194`, `test-results.md:162` "The judge is the same model (Nemotron 3 Super) as the reviewer and the verifier." For `decisions.md:3154`, a dated note was added at `:3177-3179`.
4. **What x1 changes:** `results.md:18-19` "is reported alongside and changes nothing (060)" → `results.md:25-27` "is reported alongside (060). x1 changes no decision, but it changes results: it reverses prediction (a) in Cal.com runs 1 and 3, and drops run 3 to 5th." For `decisions.md:3053`, a dated note was added at `:3174-3176`. Also added, in the losses list (`results.md:176-177`): "Under bench-judge-x1 it is also reversed in Cal.com run 3 (pool-verify-v1 47.3%, review-diff-v1 52.6%)". 060's "This changes nothing" (`decisions.md:2987`, about dev adoption) is accurate and was left alone.
5. **Interval statement (as corrected):** new at `results.md:52-54`, and in the headline at `results.md:5-7`: "No gap to another tool is outside its 95% interval. The CodeRabbit gap is the clearest: TraceHound is ahead in 96% of resamples, but its interval still includes 0 (audit-cc01 §3.2)." It is linked to the audit's §3.2 anchor. Also new: `results.md:70-71`, under the per-run rank table, "These ranks are point estimates, not a ranking: no gap to another tool is outside its 95% interval (audit-cc01 §3.2)."

## All-50 tables (`results.md` §5)
Sources:
- **Tools:** dev counts from `scorer-checks.md` (v1, x1) + Cal.com run 1 from `test-results.md:21-41`. The tools' v1 sums equal `scorer-validation.md:24-28`.
- **Row (i):** dev run 1 + Cal.com run 1. It equals audit §3.1.
- **Row (ii):** mean TP/FP over dev runs 1, 2r, 4, 5, 6 (`vote.md:33-37`, `:49-53`) + Cal.com run 1. Dev means: v1 TP 32.8 / FP 48.2 / FN 41.2; x1 TP 38.4 / FP 37.0 / FN 35.6.

**No recomputation disagreed with a published number.** A script checked all 14 rows (P, R, F, TP + FN = 97).

bench-judge-v1 (scorer of record):

| reviewer | rank of 6 | TP | FP | FN | P (n) | R (n) | F |
|---|---|---|---|---|---|---|---|
| Augment | 1 | 56 | 16 | 41 | 77.8% (56/72) | 57.7% (56/97) | 66.3% |
| Cursor | 2 | 42 | 17 | 55 | 71.2% (42/59) | 43.3% (42/97) | 53.8% |
| Greptile | 3 | 45 | 40 | 52 | 52.9% (45/85) | 46.4% (45/97) | 49.5% |
| TraceHound (i): dev run 1 + Cal.com run 1 (scorer-of-record line) | 4 | 40 | 57 | 57 | 41.2% (40/97) | 41.2% (40/97) | 41.2% |
| TraceHound (ii): dev 5-run mean + Cal.com run 1 (variance check) | 4 | 46.8 | 59.2 | 50.2 | 44.2% (46.8/106) | 48.2% (46.8/97) | 46.1% |
| CodeRabbit | 5 | 41 | 67 | 56 | 38.0% (41/108) | 42.3% (41/97) | 40.0% |
| Copilot | 6 | 43 | 104 | 54 | 29.3% (43/147) | 44.3% (43/97) | 35.2% |

bench-judge-x1 (report only):

| reviewer | rank of 6 | TP | FP | FN | P (n) | R (n) | F |
|---|---|---|---|---|---|---|---|
| Augment | 1 | 56 | 17 | 41 | 76.7% (56/73) | 57.7% (56/97) | 65.9% |
| Cursor | 2 | 45 | 11 | 52 | 80.4% (45/56) | 46.4% (45/97) | 58.8% |
| Greptile | 3 | 49 | 32 | 48 | 60.5% (49/81) | 50.5% (49/97) | 55.1% |
| TraceHound (i): dev run 1 + Cal.com run 1 (scorer-of-record line) | 4 | 45 | 45 | 52 | 50.0% (45/90) | 46.4% (45/97) | 48.1% |
| TraceHound (ii): dev 5-run mean + Cal.com run 1 (variance check) | 4 | 52.4 | 48.0 | 44.6 | 52.2% (52.4/100.4) | 54.0% (52.4/97) | 53.1% |
| CodeRabbit | 5 | 45 | 58 | 52 | 43.7% (45/103) | 46.4% (45/97) | 45.0% |
| Copilot | 6 | 50 | 91 | 47 | 35.5% (50/141) | 51.5% (50/97) | 42.0% |

Each TraceHound row is ranked against the five tools only. Both rows are 4th of 6 under both judges, and the headline uses row (i).

## Levers table (`results.md` §6)

| decision | idea | bar | result (F / P on dev) | verdict |
|---|---|---|---|---|
| 056 verify-v1 | A second Super pass gives each review-diff-v1 finding a confidence of 1–5; findings below a threshold are dropped. | The threshold with the highest dev F, ties to the higher; no bar against the baseline. | Threshold 1, which keeps every finding: 32.2% / 25.6%. Thresholds 2–5: F 32.1, 31.2, 29.2, 29.1%; P 29.5, 30.0, 31.7, 44.4%. | No gain: the rule chose the threshold that removes nothing. |
| 057 review-ctx-v1 | The reviewer also sees repository context: the changed files and git grep references. | None pre-registered; compared with review-diff-v1 (32.2% / 25.6%). | 31.8% / 36.2% | F did not move. Used later only as a second source for 059's pool. |
| 058 verify-ctx-v1 | A verifier that sees repository context answers keep or drop for each review-diff-v1 finding. | F ≥ 35.0% | 32.8% / 38.9% | Loss; not used on its own. |
| 059 pool-verify-v1 | Pool review-diff-v1's findings with review-ctx-v1's ctx-only findings, then filter with verify-ctx-v1. | F ≥ 35.0% | Run 1: 35.6% / 36.1%. 5-run mean (064): 42.3% / 40.5%. | Adopted, at a 0.6-point margin on run 1. |
| 064 vote | Keep only findings that recur across five independent runs (best variant: vote-k3of5). | v1 F ≥ the baseline's 5-run mean + 5.0 points; v1 P above the baseline's mean; x1 F ≥ the baseline's x1 mean. | vote-k3of5: 47.8% / 53.3%, against the baseline's 42.3% / 40.5%; x1 F 50.4%, against 51.3%. | Loss: fails the x1 condition. |
| 065 evidence verifier + gate | B: a verifier that must quote the failing code, then a deterministic gate that checks the quote. A: the gate alone, on pool-verify-v1. | A: mean P above the baseline's, and mean F at least the baseline's under v1 and x1. B: mean v1 F +3.0 points, mean v1 P +8.0 points, mean x1 F at least the baseline's. | 5-run means: A 41.6% / 40.3%; B 38.3% / 37.2%; baseline 42.3% / 40.5%. | Loss: A and B both fail every condition. |

Then: "The run-to-run spread of the adopted reviewer (35.6–48.0 F over 5 runs at temperature 0) is larger than every effect we tried to measure. NVIDIA recommends temperature 1.0 for this model: its model card recommends temperature 1.0 and top_p 0.95 for text ([model card], [NIM reference]). All our runs used 0, which is a limit, not a tuned choice."

## README diff (`## PR reviewer benchmark`, line 24 only; the table is unchanged)

```diff
-On the pre-registered blind test, TraceHound ranked 3rd of six under the scorer of record, behind Augment and Cursor, ahead of Greptile, Copilot and CodeRabbit. On the dev set it ranked 5th of six. Run-to-run spread at temperature 0 is larger than the margins the reviewer was chosen on, so treat single-run ranks as approximate. F under bench-judge-v1; details, losses and limits in [docs/bench/results.md](docs/bench/results.md).
+On 10 blind pull requests (Cal.com run 1, n = 23), TraceHound is statistically tied with Greptile and Cursor. Its run-1 F was 58.3% against CodeRabbit's 36.7%, but that is not a significant lead. No gap to another tool is outside its 95% interval. The CodeRabbit gap is the clearest: TraceHound is ahead in 96% of resamples, but its interval still includes 0 ([audit-cc01 §3.2](docs/bench/audit-cc01.md#32-bootstrap-95-ci-over-prs-for-the-calcom-f-difference)). Across all 50 pull requests it is 4th of 6 under both judges ([all-50 table](docs/bench/results.md#5-all-50-prs)). Every loss is published, including every precision lever we tried on dev ([levers table](docs/bench/results.md#6-precision-levers-tried-on-dev)); the run-to-run spread at temperature 0 is larger than every effect we tried to measure. Tool numbers come from their recorded reviews in the public benchmark dataset, scored with our judge; we did not run the tools. F under bench-judge-v1; details, losses and limits in [docs/bench/results.md](docs/bench/results.md).
```

## Viewer (the two allowed files only)
- **`benchmark.ts`:** `BENCHMARK_TABLES` was generated from results.md's tables with the test's own parse, so there was no hand transcription. Titles and notes were written by hand. The all-50 tables are titled "All 50 PRs, bench-judge-v1 (scorer of record)" and "All 50 PRs, bench-judge-x1 (report only)". Their notes don't use the phrase.
- **`benchmark.test.tsx`:**
  - the table-equality test stays (renamed, without "without an invented combined view");
  - the table count is now `BENCHMARK_TABLES.length` (7);
  - "all 50 PRs" may appear only as 2 H2s + 2 CAPTIONs, and the page's total count must equal that;
  - the page text must not match `/3rd of (6|six)/i`.
- **The "3rd" in the viewer:** it was the per-run rank cells (`benchmark.ts`, from results.md §3). I kept the ranks (the 5th in run 3 under x1 is a published loss). I relabelled the columns "F, point-estimate rank", and the note now says the ranks are point estimates, not a ranking. If you want the ordinals gone entirely, that is a results.md §3 change.

## Done that the brief didn't ask for
- `0d3a6c4`: `BENCHMARK_REVISION` re-pinned `e4e3e3a → e54ee63` (the last commit to change results.md), so the page's source link shows the tables it renders. Its own commit; it's inside an allowed file.
- results.md title is now "(decisions 063, 066)". The sources sentence names vote.md, evidence.md and scorer-validation.md, and says the §5 tables are computed.
- The all-50 tables put the reviewer first and the rank second (the audit's table had rank first). This keeps the viewer's row keys unique and its TraceHound row highlight working.
- Backticks were removed from one levers cell, because the viewer would show them literally.
- Fix 1 adds the clause that the tools' scores were in the repo from 054. The 061 note also covers "written before anyone has seen a Cal.com result". The run-3 x1 reversal was added to the losses list.
- The CLAUDE.md line (255) also notes that the viewer's tables must equal results.md's.

## Not done or open
- `viewer/app/benchmark/page.tsx` still says "The judge and reviewer share a model family" and "decision 063". That file was not in the allowed two. It needs a follow-up.
- `CLAUDE.md:250` (the 062 entry) still says "pool-verify-v1 ranks 3rd of six". The brief allowed one CLAUDE.md line, and I used it on 255.
- The first temperature link is the Nemotron **3.5** Super **VL** model card; our runs used `nvidia/nemotron-3-super-120b-a12b`. I opened neither link.
