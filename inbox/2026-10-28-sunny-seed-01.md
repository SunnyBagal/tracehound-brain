# seed-01: final report (TraceHound, seeded test)

Results committed in tracehound-seeds 88a43c6 (RESULTS.md, raw/). Scorer, key.json and SCORING.md unchanged (hashes re-checked at scoring).

## Step 0: all pass
- a4a6c83 in history; key.json and SCORING.md sha256 match.
- Scorer offline checks 13/13; scorer makes no model calls.
- ADMIN on both mirrors; NEBIUS_API_KEY present on both; default branch main; no workflow and 0 PRs beforehand; seed branches 15/15 each.

## Commits
- linea-seeded 3012a54: TraceHound workflow (0aeef06). 6d8771a: uses: line repinned to 86fd5d7 (review-freeze-2.1).
- recall-seeded 296f8f3: TraceHound workflow (0aeef06).
- tracehound-seeds dbd3bdd: runs/open-prs-seed01a.sh (disclosed copy). 1fc811b: ONLY_REPO filter added. 88a43c6: RESULTS.md + raw/.

## Per mirror
| | Recall | Linea |
|---|---|---|
| Action | 0aeef06 (review-freeze-2) | 86fd5d7 (review-freeze-2.1) |
| PRs opened | 15 | 15 |
| Final runs green | 15 | 15 |
| Red runs | 0 | 3, all on linea#1 (2 at 0aeef06 incl. 1 rerun; 1 re-trigger still on 0aeef06) |
| Partial reviews | 0 | 0 |
| Reviews posted | 8 | 5 |
| Green, no review posted | 7 | 10 |
| TraceHound comments | 13 | 8 |

## Score (automatic rule; manual column +0, manual.json empty)
| Class | Overall | Recall | Linea |
|---|---|---|---|
| X-typed | 2/6 | 2/3 | 0/3 |
| X-keyed | 2/6 | 1/3 | 1/3 |
| X-semantic | 0/4 | 0/2 | 0/2 |
| all X | 4/16 | 3/8 | 1/8 |
| D | 5/6 | 3/3 | 2/3 |
| all seeded | 9/22 | 6/11 | 3/11 |
| Control false alarms | 0 (0 comments on 8 controls) | 0 | 0 |

- Comments per PR: 0.70 overall (21 on 30 PRs).
- 3 of the 4 X catches came from the review's dependents section (contracts) in 0-findings reviews.
- 2 manual-call candidates for Sunny are listed in RESULTS.md.
- All 8 controls: no review posted.

## Green, no review posted (reviewer-step duration)
- Recall: #1 13.5 s, #5 29.5 s, #8 40.0 s, #11 32.5 s, #13 48.1 s, #14 132.2 s, #15 23.0 s
- Linea: #1 63.2 s, #3 7.5 s, #5 51.9 s, #6 19.0 s, #7 6.7 s, #9 56.3 s, #10 22.2 s, #11 6.7 s, #14 14.7 s, #15 55.1 s

## Video links
- Catch: https://github.com/SunnyBagal/recall-seeded/pull/7#pullrequestreview-5673943375
- Catch: https://github.com/SunnyBagal/recall-seeded/pull/6#pullrequestreview-5673929237
- Catch: https://github.com/SunnyBagal/recall-seeded/pull/4#discussion_r4393814033
- Miss: https://github.com/SunnyBagal/recall-seeded/pull/12

## Deviations
- CodeRabbit, Greptile and Bugbot not run.
- TraceHound on Nemotron-3.5-Super-VL (not 3 Super); Linea on review-freeze-2.1 (install fix only).
- Linea seeds opened after the Action fix, same order otherwise.
- Open-PR script copy with 4 disclosed changes (no CodeRabbit trigger, 90 s gap, cd to repo root, ONLY_REPO).
- linea#1: base re-set to main once (no content change) plus 2 draft/ready re-triggers.
- Devin Review auto-reviewed the first 3 PRs (6 comments, not scored), then removed.
- First-PR gate widened to 3 PRs.
- RESULTS.md force-added (.gitignore's results.md matches it case-insensitively).

## Not available
- Nebius spend: not shown in the run logs.
