---
updated: 2026-10-20 19:10 IST
updated_by: chat
---

# Now

**Latest handoff: [history/TraceHound_Handoff_Oct20.md](history/TraceHound_Handoff_Oct20.md).** Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- **TraceHound main: **`b98f753`** (verified 19 Oct 13:55, #45 merge). Earlier: `5857179` (#47), `a9d16b5` (#46), `2af8c3a` (#43).** Before that `c6c12b9` (#44 merge). Older: main `e7309a5` (verified 18 Oct 00:10): #33 merge `6b4276e`, #34 merge `e7309a5`. Tag `review-freeze-1` → `6b4276e` (#33's merge commit), tagged 17 Oct 22:14 IST. #35 merged 18 Oct (`3113c3a`). #36 → `d37ffa8`, #32 → `e95ef20`, #37 → `df4f915` (all merged 18 Oct). #38 → `41a92a8` and #39 → `6058d53` (both merged by Sunny 18 Oct ~20:25, merge commits; #38 before codex-06c, by accident; harmless: suggestions work, #2's just isn't committable). #40 merged (`e4e3e3a`).
  Open:
  - **#43** codex-07 data branch, head **`19ece32`** (12 commits; ur-01 fixes on top of `d733a6a`). **Re-checked by chat 19 Oct 00:10:** source diff read against ur-01; #1 merge-base diff, #2 monotonic latest + state rank, #3 ref-lock retry, #4/#8 split `git`/`branchGit` from `cleanGitEnv()`, #9/#10 last-line footer + JSON temp file all match; #5 backlog line; #6/#7 example split into review (cancel-in-progress) and data (serialized, only `contents: write`) jobs. Action tests 153/153 and typecheck re-run locally; merges cleanly with main `c6c12b9`. `max-comments: '0'` returns before any GitHub/Nebius call (verified in `action.ts`). Accepted departures: reopened → open; #5 own commit; extra re-run row. **Merged 19 Oct** (`2af8c3a`). Residuals, backlog only: a manual re-run of an old `reopened` after a later close flips the entry back to open; a late event for a new head after close adds an open entry; dropped pending data jobs can leave a closed PR `open` (documented); the split example records no `reviewFooter` (viewer doesn't read it today; ui-03 must not depend on it). Dogfood pin still `e4e3e3a`.
  - **#44** ui-02 dashboard: **merged** (main `c6c12b9`, verified 19 Oct 00:05). Re-sync `viewer/lib/benchmark.ts` after the R6 wording fixes land (it still shows "3rd").
  - #41 (cc-01 audit) and #42 (ReviewBench spike) are drafts; their content feeds later jobs. Next relay: **064 voting across runs** (`064-vote.md`, cap $10, dev only, PR as draft). Next Codex: `codex-06c-deleted-lines.md` (rewritten against main, new branch `codex/fix-committable`).
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $39.93** (18 Oct 21:00; was $46.22 on 17 Oct; −$6.29 = 062's $5.31 + about $0.98 of off-ledger CI: dogfood + demo live checks); ledger **$23.17092** after 064 (19 Oct; was $12.45236). Caps: total stays at the $45 default (ledger + $15 fits; decided 17 Oct), per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

## In flight

#33 and #34 merged with merge commits: **verified on GitHub 18 Oct**.

- **#35** `ui/viewer-round-1` head `acc5752`, 18 commits: checked by chat 18 Oct. Lane paths only; $0 Nebius; no deps; snapshots at Recall `20ac149` and demo `efc5844` = their mains; CI green (the prisma annotation comes from the analyzer fixture and was already there). One red intermediate commit (`f6df5a9`), disclosed. Nits go to ui-02: edge labels collide, titles truncate, "1 modules". Step 9 now goes to a **new branch** via `ui-01b-after-062.md`.
- **062 done → PR #36** (head `9c0624f`, CI green). Checked by chat 18 Oct 03:40:
  - all 16 table rows recompute, and the spreads are consistent;
  - frozen paths are unchanged against `review-freeze-1`;
  - the predictions match 061's wording;
  - no Cal.com text.
  - Relay's BLOCKED was only "main moved" (#35), which is harmless. `tracehound/025` is relay's own job counter (brief 062).
  - **Headline: blind Cal.com run 1, v1: pool-verify-v1 F 58.3% (P 56.0 / R 60.9), 3rd of 6** (augment 73.2, cursor 66.7 | greptile 50.0, copilot 46.2, coderabbit 36.7). It was 3rd in 5 of 6 run×judge cells and 5th in x1 run 3 (47.3).
  - Predictions: (a) right, (b) wrong, (c) right.
  - Dev spread: 35.6 → 48.0 across two runs (larger than the 0.6-point adoption margin).
  - Dev run 2 was void (relay's 2 h background limit killed it), then 2r; dev run 3 was stopped at the cap.
  - Spend $5.31; **ledger $17.76**.
- 063 → PR #39 (relay 026). Checked by chat 18 Oct: 4 small commits; ranks, spreads and costs trace to the repo.
  - Wording nit for the judges' README job: section 1 says x1 "changes nothing". It means it decides nothing; x1 reverses (a) and gives a 5th in run 3, both stated in §3/5/6.
  - Relay lane is idle until the judges' README (21–22 Oct).
- **Dogfood spend is off the local ledger** (`secrets.NEBIUS_API_KEY` in CI), roughly $0.03–0.25 per PR. Decided by chat: keep it on (it's live demo evidence) and track it via the Nebius console. Any future job that needs "no other Nebius use" opens its PRs as drafts (the workflow skips drafts).

| Lane | Job | Status |
|---|---|---|
| Relay | **062** Cal.com run (`briefs/062-test-run.md`, cap $15): fetches → tag `review-freeze-1` on #33's merge commit → Cal.com run 1 (headline) → runs 2–3 → dev runs 2–3 → results doc | Starting 17 Oct night. Needs `nebiusCapUsd` = 15, #32 unmerged, no other Nebius use. Takes hours |
| UI lane | **ui-01-codex.md**: round 1 + addendum merged, now **Codex GPT Sol 6.1** in ~/Projects/TraceHound-ui, branch `ui/viewer-round-1` | Starting 17 Oct night. No Nebius until Sunny writes "062 is done"; step 9 (Super summaries, $0.20) comes after |
| Codex | #32 dogfood workflow + demo doc | **Hold until 062's PR is done** |
| Codex queue | codex-06 fix suggestions → codex-07 data branch (persistence) | after 062 (its Action runs use Nebius) |
| Grok | grok-04 X post draft | Post Mon 19 Oct; Cal.com numbers only once 062's results doc has merged and the chat has checked it |

## Known problems

- **Codex automatic review** has posted nothing on #27–#31. Check the Review trigger, or comment `@codex review`.
- **Demo (17 Oct, pin `50e1d4a`):** the reviewer commented on the seeded line in 2 of 4 demo PRs (#2, #3), missed #1 and #4, and posted 0 comments on the control (#5). Dogfood #32: 1 comment.
- **No live reviewer demo yet**, and judges may score from the video and text alone (R3).
- **X post planned for Monday 19 Oct** (benchmark thread; tags to verify).

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 done (adopted) | #28 merged |
| 17–18 Oct | 060 scorer fairness: augmented precision plus a second judge | Demo repo with the action installed and real review comments (the judges' working demo); switch the action to the reviewer that wins |
| 20 Oct | Results doc (moved earlier) | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the one Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md, "what changed since 26 August" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct (deadline 30 Oct 22:30 IST) | |

## Open decisions

- [P004](decisions/P004-target-reset.md): **rejected 18 Oct.** "Beats" is allowed, scoped to the test, run and scorer. The goal is the top 2 blind (Cursor 66.7 / Augment 73.2 on Cal.com). **18 Oct chat update:** the blind result (3rd of 6, ahead of Greptile in 5 of 6 cells) makes the honest framing *stronger*, not weaker; still no "beats" headline because of the spread.
- Decided 2026-10-16 (Sunny): (a) first, as 059 pool-verify-v1. Next after it: 060, augmented precision for every reviewer plus a second judge.
- Decided 2026-10-16 (Sunny): the Action switches to pool-verify-v1; the 060 second judge is Qwen3-235B-A22B-Instruct-2507; 060 covers the second judge plus augmented precision.
- Decided 2026-10-16 (Sunny): **stop tuning, freeze now**; demo on **both** TraceHound itself (dogfood) and a seeded-bug demo repo.
- Cal.com run: pool-verify-v1 primary. Chat recommends declaring diff-v1 and pool-raw as secondary rows in advance; they come free from the same pipeline.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30, and decisions 001–031 first appear that same day, so they were likely carried over from earlier work. Is the earlier history (FixFlow) somewhere public? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.

## v2 architecture (19 Oct, proposed, Sunny to approve)

See [project/architecture-v2.md](project/architecture-v2.md): S1 pre-filter → S2 contract map → S3 generators → ~~S4 vote~~ (lost in 064) → S5 evidence verifier → S6 post-gate → S7 deliver → S8 learn. Freeze-2 is on 24 Oct, followed by **one** blind Cal.com run of v2. **No standalone Cal.com run for 064** (nothing was adopted). Ultrareview #2 moves to the int-01 branch. Codex and UI end 22 Oct; Claude B takes integration and UI.

## Open PRs (checked by chat 19 Oct 03:40; all three merge cleanly with main and with each other; merged together locally, analyzer 724 tests pass and typecheck is clean)

- **#46** 064 vote, head `d76eeb3`, draft, CI green. **Loss, recorded as one. Merge it** (mark it ready, which runs the dogfood review for a few cents; use a merge commit).
  - Every table recomputes: comments = TP+FP+dup+low and TP+FN = 74 in all 18 rows.
  - The means recompute: v1 F 42.3 / P 40.5; x1 F 51.3 / P 50.9.
  - Commit order: plan `57f0b91` at 21:09 IST, then `vote.ts` at 21:13 IST, both before any run finished.
  - Accepted: two test files under `packages/analyzer/test/`.
  - **Not accepted:** the builder edited `tracehound-ops/ops/lanes.json` (cap 0 → 10). 065's brief now forbids any edit to tracehound-ops. Sunny: check what lanes.json says.
- **#45** codex-08 gate (+08b): **merged by Sunny 19 Oct 13:46 → main `b98f753`**. Chat re-ran the full suite on main afterwards: action 153, analyzer 778 (+7 skipped), viewer 186; typecheck clean; the `gate` usage line has `--base`, and `contracts` prints its usage. Main CI run 39221589986 was in progress at 13:55.
- **#41** cc-01 audit: **merged** (in main `9a81505`).
- **#48** 065 evidence verifier + gate, head `d2e5a6b`, draft, CI green. **A loss, recorded as one. Merge it** (mark it ready, merge commit). Checked by chat 19 Oct 16:20: all 40 table rows recompute; means v1 baseline F 42.3 / P 40.5, A 41.6 / 40.3, B 38.3 / 37.2; x1 baseline 51.3, A 50.4, B 45.7. Freeze `9bfb3eb` came before the driver and the first call, and the diff after it is empty. Spend $2.651 → **ledger $25.82**. The evidence checks dropped TPs at the pool's base rate (13 of 59), and contracts made 0 findings on dev (mostly non-TS repos).
- **#42** ReviewBench spike (draft, `scripts/reviewbench/` + doc): **leave as a draft** until Sunny decides on ReviewBench (it needs Anthropic API credit: ~$3 for the practice set, ~$7 for a 60-PR run). If it's a no by 22 Oct, close it unmerged; R7 keeps the findings.
- **#47** cc-03 contracts, head `d469e28`, draft. Paths are in scope; 141 tests; 10 breaks caught, 0 findings on the 10 controls; 0 findings on 9 real commits; excalidraw takes 53–65 s against a 120 s budget. **Merge** (mark it ready). Known gaps are written up: package types are `any` without node_modules, and route mount prefixes are ignored.

**Blindness (from 19 Oct):** no TraceHound lane (relay, Codex, Claude B, cloud builders, Grok) and not the chat may open `linea-seeded`, `recall-seeded` or `tracehound-seeds` until scoring starts on 25 Oct. Every brief from now on says so.

## Credit (20 Oct 18:50)
**Claude cloud: $52** (Sunny). It was ~$85 at 18:00. This planning chat may be a cloud session too; Sunny is checking per-session usage. From now on: rb-02 and int-01 go to **relay** (Pro plan), and the cloud is only for rb-01b. The chat keeps PR checks lean.

## HOLD #49 (20 Oct 19:05): codex-09b first
Checked by chat on `30a3d44`. Paths are in scope; analyzer learn tests 47/47 and action tests 174/174 pass; `learn: 'off'` is unchanged; the permission check is right.

**Defect** (it came from the brief): code spans are dropped, so identifiers vanish. The probes:
- a rejected null-deref on `user` suppressed a null-deref on `order` in the same file;
- it also suppressed a template-alike comment in another `.ts` file.

Fix: [codex-09b](prompts/2026-10-20/codex-09b-learn-fixes.md), in the same Codex task. **Must land by 22 Oct.**

Grok bots: [grok-09](prompts/2026-10-20/grok-09-bots.md) has three jobs:
- A: ReviewBench portal questions. Includes who pays the judge, and whether we can re-register a digest for candidate 6.
- B: X and LinkedIn drafts, with the Verifier checking numbers against results.md.
- C: a daily digest.

## HOLD #50 (20 Oct 18:55): don't merge until rb-01b lands
Checked by chat on head `1d06203`:
- 27/27 adapter tests pass on Node 22.
- The never-fail wrapper, the 40-finding cap and the contract schema check are fine.
- `golden/` is banned in the docs.
- The image workflow run #1 is green. **Sunny:** read its summary for the digest and whether the package is public.

Gaps:
1. **No 3.5 price.** The $5/$15 placeholder × 32k `max_tokens` breaks the $1 per-process cap, so every 3.5 call is refused and the run silently scores 0.
2. **The smoke test can't fail.** The wrapper always exits 0.
3. The smoke test has no model input.

Fix: [rb-01b](prompts/2026-10-20/rb-01b-nemotron-3.5.md), in the same cloud session as #50.

**P008 amendment 2 (decided by the chat, Sunny delegated):** candidate 6 is `pool-raw-missed`, built by [rb-02](prompts/2026-10-20/rb-02-missed-issue-pass.md) (new cloud session, after #50 merges). Cutoff 23 Oct 12:00 IST.
- **No ReviewBench test run of any candidate starts until the chat has recorded rb-02's prompt sha256 in P008.**
- If rb-02 misses the cutoff, drop candidate 6 and start the runs.

## grok-08 (20 Oct 18:20)
- Filed → [R11](research/R11-grok08.md). Kill test 5/5 real, 3 overstated. **No P008 amendment**: its top 3 are `pool-raw-x2` (already candidate 3) or new prompts/code. Adds one rb-01b check: 3.5 smoke must report `finish_reason` (reasoning shares `max_tokens`). **ChatGPT pass (18:35):** kill test 5/5 (LAURA misattributed to callers/callees; Qodo extended numbers only in charts). New: **missed-issue pass** (CR-Bench: +5.75 recall on GPT-5.2, closed models, iterative). Chat recommends **no** candidate 6; **Sunny decides before any test run**. rb-01b also checks `learn` off in the image.

## ReviewBench (19 Oct 18:20): the main lever toward "above Greptile and Cursor"
- 219 PRs, run by GitHub + Microsoft; grounded F1 is driven by recall; Greptile 27.2, Cursor 17.3, Copilot 40.1 ([R7](research/R7-cc02-reviewbench.md), [R9](research/R9-reviewbench-methodology.txt)).
- **Pre-registered: [P008](decisions/P008-reviewbench-preregistration.md).** Never open `golden/`. Choose between pool-verify-v1, pool-raw and pool-raw-x2 from the 25-PR test runs only; then one final run.
- Prompt `rb-01-reviewbench-image.md` (cloud session): adapter, Dockerfile, ghcr build, smoke workflow. Then Sunny: portal registration → approval → 3 test runs → final.
- **Nemotron 3.5 Super is on Token Factory at the same price** as 3 Super (SWE-bench Verified 71.1 vs 60.5); see [R10](research/R10-smarter-reviewer.md). It was added to the ReviewBench candidates (P008 amendment 1) and to rb-01's support list. Fine-tuning isn't possible: Token Factory post-training has no Nemotron.
- Needs a Nebius top-up of about $25. Sunny has Nebius **sandbox** access (beta); it's not needed for ReviewBench, but it's a candidate for "checked fixes" later.

## Prompt queue (19 Oct 03:45)

| Lane | File (prompts/2026-10-19/) | Where | Status |
|---|---|---|---|
| Codex | `codex-08b-gate-fixes.md` | the **same** Codex task as #45 | ready |
| ~~Relay~~ | ~~065~~ → **#48, a loss** | | done |
| Relay | `066-results-docs.md` (R6 fixes, all-50 with a dev 5-run-mean row, levers table, README headline; **$0**) | `~/Projects/tracehound-ops` → TraceHound | after #48 merges; Sunny sets `nebiusCapUsd` = 0 |
| Codex | `codex-09-learning.md` (S8: 👎 / resolved-unchanged / reply → suppress; maintainers only; `learn` input default off; conventions reader, not wired) | ChatGPT Codex, `~/Projects/TraceHound-codex`, new task | **ready, must land by 22 Oct** |
| (old) Relay | `065-evidence-gate.md` | Run 1 stopped at step 0 at 13:00 IST ($0). Brief patched. **#45 is merged now, so it's ready to rerun**: re-copy the brief from the brain, then start. lanes.json = 5 (confirmed by relay) |
| ~~Grok~~ | ~~grok-07~~ → [R8](research/R8-competitors-learning.md): links recovered and 5 of 5 checked. **Sunny keeps CodeRabbit** (seeded test + public numbers) despite §4.2(iv) of its terms. Seeded-test tools: TraceHound, CodeRabbit, Greptile, Bugbot (Bugbot ≈ $1–1.5 per run) | | done 19 Oct |
| Cloud | ~~cc-04 + cc-04b~~ **done 17:40**: `tracehound-seeds` `a4a6c83`. Scorer has 13/13 offline checks; `open-prs.sh` triggers CodeRabbit and opens one PR per 450 s (≈3 h 40 m for 30). **Pre-registration (published 19 Oct 17:50 IST):** `sha256(key.json) = abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1`, `sha256(SCORING.md) = ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d`. 25 Oct: install the TraceHound Action (freeze-2 pin), CodeRabbit, Greptile and Bugbot on both mirrors, then Sunny runs `open-prs.sh` | | done |
| Codex | codex-09 learning (S8) | — | after codex-08b |
| Claude B | int-01 integration (`th-v2` in the Action) | `~/Projects/TraceHound-claude2` | 22 Oct |

## Prompt queue (18 Oct 21:45). Every lane busy, no path overlaps

| Lane | File | Where | Waits on |
|---|---|---|---|
| Relay | `064-vote.md` (voting across runs, dev only, cap $10) | `~/Projects/tracehound-ops` → TraceHound | `nebiusCapUsd` = 10 |
| ~~codex-07~~ → **#43** | | |
| ~~ui-02~~ → **#44** | | |
| ~~cc-01~~ → **#41** draft, [R6](research/R6-cc01-audit.md): numbers all correct; 5 wording overclaims in results.md; gaps within noise except CodeRabbit | | |
| ~~cc-02~~ → **#42** draft, [R7](research/R7-cc02-reviewbench.md): probably feasible; 25-PR practice set ≈ $4; judge needs Anthropic API money | | |
| Grok | ~~grok-05~~ → [R4](research/R4-precision-techniques.md) (done 18 Oct; Cursor's early Bugbot = 8-pass vote + validator) | | |
| Grok | ~~grok-06~~ → [R5](research/R5-hackathon-and-nebius-levers.md) (done 18 Oct; Coding track favours plan → test → iterate agents; weights unknown) | | |

Path guard: relay owns `scripts/bench/`, `docs/bench/` except the cc-01/cc-02 files, and `docs/decisions.md`; Codex owns `packages/action/`, `.github/`, `docs/action/`; UI owns `viewer/`, `docs/ui/`; cc-02 owns `scripts/reviewbench/`.

## Credits
- Nebius console $39.93 (18 Oct 21:00).
- **Ultrareview: 3 free runs** (Claude cloud bug-finding agent, about 15–25 min each). Spend them only on branches before merge, highest risk first:
  1. codex-07 branch (adds `contents: write` and pushes the orphan branch; touches other people's repos);
  2. 064 branch (voting and scoring code that the headline number depends on);
  3. reserve: the final pre-submission branch (27–28 Oct), or ui-02 if it renders untrusted comment text.
  Never on main, and never on docs-only PRs.
- Claude cloud: **$89 of $100 left, expires 5 Nov 13:29 IST**. It's cloud-session credit only (screenshot 18 Oct), **not API credit**, so it can't pay the ReviewBench judge (Claude Sonnet 5 API). Judge cost is about $3 for the 25-PR practice set and about $7 per 60-PR run; that needs a separate Anthropic API top-up (Sunny's call).
- (Superseded line follows.) Claude cloud $96, expires 5 Nov. Spend it on audits, spikes, judges' README drafts and TariffLens setup after 29 Oct.

## Honest claim status (18 Oct, revised after cc-01)
- **Public line:** "on 10 blind PRs, statistically tied with Greptile and Cursor, ahead of CodeRabbit; 4th of 6 across all 50 PRs; every loss published." Don't say "3rd of 6" as a ranking.
- Blind Cal.com: 3rd of 6 (10 PRs, n = 23).
- All 50 PRs: **4th of 6** (41.2 vs Greptile 49.5, CodeRabbit 40.0).
- Sunny does not want claims that draw "how did you beat CodeRabbit?" questions, so the post wording is "3rd of 6 blind, 4th of 6 across all 50, losses published".
- The top-2 push (064) is judged on all 50 PRs, not on Cal.com alone.
