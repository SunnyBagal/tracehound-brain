# codex-18: seeded slots filled, 28 Oct 2026

Both requested branches were pushed. Docs only; $0 model spend; no model calls. No seeded repository code, PR diffs or review bodies were opened. Source was only `submission/numbers.md` sections 0 and 7; the supplied PR links were copied directly.

## PRs and files

- [PR #54](https://github.com/SunnyBagal/TraceHound/pull/54), `docs/judges-readme`: `6c6fb954fece8337813681a54f3619ae6221b304`. Changed `README.md` only.
- [PR #58](https://github.com/SunnyBagal/TraceHound/pull/58), `docs/devpost-drafts`: `32833abb11cf3db5bea3011f4543b0f379e98bfa`. Changed `docs/devpost/writeup.md`, `docs/devpost/checklist.md`, `docs/devpost/video-script.md` only.
- Local and origin heads match; product worktree is clean. Neither PR was merged. No readiness command was issued. GitHub's final read reports `isDraft: false` for both PRs; their readiness state was not changed by this work.

## Step 0

- `tracehound-brain`: `git pull --rebase` succeeded, updating to `0de8c03`. Repeated before adding this report: already up to date.
- `gh repo view SunnyBagal/tracehound-seeds --json visibility`: `PUBLIC`.
- Product checkout: `git fetch origin` succeeded; clean tree.
- Switched to each requested branch and verified HEAD equals its origin branch before editing. `git merge origin/main` on each: already up to date, no conflict.
- Sandbox denials for source pull, shared worktree metadata and GitHub network access were retried with explicit tool escalation. No work was performed in another product lane checkout.

## Exact allowed sentence

On 30 pre-registered seeded PRs, TraceHound caught 9 of 22 seeded defects and stayed silent on all 8 clean controls. It caught 5 of 6 bugs inside the diff but only 4 of 16 that break files the PR didn't touch, and 3 of those 4 came from the deterministic contract check, not the model.

## Every replaced slot: old → new

All old seeded markers below were `[[TBD-SEEDED]]`.

| file / slot | old → new |
|---|---|
| README / Seeded pull requests on the live config | marker → exact allowed sentence above; verbatim class/caught/Recall/Linea table including controls; pre-registration, committed automatic scorer and RESULTS.md link; deterministic split; not-run reviewers; model and Action refs; compact deviations |
| README / Live demo | marker → `Seeded PRs on the current config: 30 on SunnyBagal/recall-seeded and SunnyBagal/linea-seeded (results above).` with both repository links |
| writeup / Open slots | marker bullet → removed |
| writeup / Trying it without a key | marker → `the 30 seeded PRs on two public mirror repos`, with Recall and Linea links |
| writeup / Seeded PRs on the live config | marker → exact allowed sentence above, pre-registration/scorer/link, `no other reviewer was run on these PRs`, model/refs and deviations |
| checklist / Open slots table | marker row → `Seeded PRs`, scored and done `seed-01, 28 Oct` |
| checklist / row 5 | open seeded marker → seeded slots done `seed-01, 28 Oct`; README still awaits Sunny's merge |
| checklist / row 8 | seeded marker → done `seed-01, 28 Oct`; pool-verify-v1 on 3.5, Recall `0aeef06`, Linea `86fd5d7`, model and deterministic catch links, 3-of-4 split, no other reviewer run |
| checklist / row 9 | shots waiting on seeded marker → seeded script slots done `seed-01, 28 Oct`; shoot/upload still pending |
| checklist / row 15 | open seeded marker → seeded slots done `seed-01, 28 Oct`; Sunny slots remain |
| video / Before shooting | seeded marker → model catch and deterministic catch links from section 7, plus split, registration, scoring, not-run/model/refs and deviations |
| video / shot 2 | seeded marker → [check-then-act race line comment](https://github.com/SunnyBagal/recall-seeded/pull/4#discussion_r4393814033), narrated as Nemotron 3.5 Super; short [renamed-route beat](https://github.com/SunnyBagal/recall-seeded/pull/7#pullrequestreview-5673943375), narrated as `the deterministic contract check, no model`, with the 3-of-4 split |
| video / shot 3 | seeded marker → same recall-seeded PR on dashboard; if unavailable, show GitHub review; exact requested note `[[UI: recall-seeded on /reviews if #65 has the flag on]]` |
| video / shot 6 | seeded marker → exact allowed sentence above, on screen and narrated; provenance/not-run/model/refs and RESULTS.md link on screen |

Additional requested edits: README Losses now includes `The seeded test: 4 of 16 cross-file defects caught (0 of 4 semantic), 3 of them by the deterministic contract check.` Checklist row 7 now links both public mirrors and marks seeded viewing done while retaining Sunny's hosting question. Checklist row 6's stale pin now matches section 0: `review-freeze-2.1` (`86fd5d7`), installation only. Existing prohibited-word examples were removed from the writeup rules, and the README's existing selection-bias sentence was shortened to remove a prohibited word. The checklist's existing competitor name was removed. All `[[SUNNY…]]` text is byte-for-byte unchanged.

## Checks

| requested check | result |
|---|---|
| `grep -c "TBD-SEEDED"` in README.md | `0` |
| same in writeup.md | `0` |
| same in checklist.md | `0` |
| same in video-script.md | `0` |
| `git grep -n -i -E "cursor\|augment\|greptile\|coderabbit\|copilot\|bugbot"` on #58 writeup and video | prints nothing (exit 1, expected no matches); actual command used ERE alternation without escaped pipes |
| prohibited superiority phrases in all four edited docs | no matches |
| README seeded table | verbatim section 7, including controls |
| allowed sentence | verbatim section 7 in README, writeup and video shot 6 |
| Sunny placeholders in each edited file | unchanged, checked against originals |
| `git diff --check` on each new commit | passed |
| video timing | existing shot boundaries retained, total 170 seconds (2:50), below 3:00; narration shortened elsewhere |

`grep -c` exits 1 when the match count is zero; those exits are successful checks here.

Numeric provenance for additions (section 7):

- Result counts: `30`, `9`, `22`, `8`, `5`, `6`, `4`, `16`, `3`, plus `0`, `1`, `2`, `11` in the table and split/deviations.
- Exact table ratios: `5/6`, `3/3`, `2/3`, `2/6`, `0/3`, `1/3`, `0/4`, `0/2`, `9/22`, `6/11`, `3/11`, `0/8`.
- Dates: `19 Oct`, `28 Oct`; job `seed-01`; model `3.5` versus `3 Super`; pipeline `pool-verify-v1`; Action tag `review-freeze-2.1` and pipeline tag `review-freeze-2` (also in section 0).
- Commit identifiers: `a4a6c83`, `0aeef06`, `86fd5d7`.
- Link identifiers: PR `4` / `discussion_r4393814033`, PR `7` / `pullrequestreview-5673943375`; deviations include `linea#1`, script `4` changes, first `3` PRs, gate `3` PRs, base reset once and retriggered twice.
- No new quantitative claim absent from section 7. Existing timing, checklist row numbers, deadlines and surrounding benchmark claims were retained. The literal `#65` in the expressly required UI note comes from the user's instruction, not section 7; it is the sole new numeral absent from section 7 and is an issue reference, not a result. This is an explicit qualification to the requested literal every-number check.

Local gates ran in plain shells on each branch. The required GIT environment grep printed nothing before each gate. Tests made no requested external/model calls; no new tests or dependencies were added.

| branch | pnpm test | pnpm typecheck |
|---|---|---|
| docs/devpost-drafts | passed: action 249, analyzer 1,004, viewer 188; total 1,441 passed, 8 skipped; 96 test files passed, 6 skipped | passed: 3 workspace projects, 0 errors |
| docs/judges-readme | passed: action 249, analyzer 1,004, viewer 188; total 1,441 passed, 8 skipped; 96 test files passed, 6 skipped | passed: 3 workspace projects, 0 errors |

## Not done

No merge, publication to Devpost, video recording/upload, dashboard flag verification, hosting verification or new review run. Sunny placeholders remain. Both PRs' readiness states were left untouched. No model calls or new spend. The dashboard has the requested GitHub fallback; its live recall-seeded display was not tested.
