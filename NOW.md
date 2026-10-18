---
updated: 2026-10-18 20:35 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `e7309a5` (verified 18 Oct 00:10): #33 merge `6b4276e`, #34 merge `e7309a5`. Tag `review-freeze-1` → `6b4276e` (#33's merge commit), tagged 17 Oct 22:14 IST. #35 merged 18 Oct (`3113c3a`). #36 → `d37ffa8`, #32 → `e95ef20`, #37 → `df4f915` (all merged 18 Oct). #38 → `41a92a8` and #39 → `6058d53` (both merged by Sunny 18 Oct ~20:25, merge commits; #38 before codex-06c, by accident; harmless: suggestions work, #2's just isn't committable). No open PRs. Next relay: **064 voting across runs** (`064-vote.md`, cap $10, dev only, PR as draft). Next Codex: `codex-06c-deleted-lines.md` (rewritten against main, new branch `codex/fix-committable`).
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $39.93** (18 Oct 21:00; was $46.22 on 17 Oct; −$6.29 = 062's $5.31 + about $0.98 of off-ledger CI: dogfood + demo live checks); ledger $12.45236. Caps: total stays at the $45 default (ledger + $15 fits; decided 17 Oct), per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

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
