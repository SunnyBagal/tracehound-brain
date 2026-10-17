---
updated: 2026-10-17 22:10 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `f437ecb` (#30 Action on pool-verify-v1 and #31 060 merged 2026-10-16/17). No open PRs.
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $46.22** (2026-10-17); ledger $12.45236. Caps: total stays at the $45 default (ledger + $15 fits; decided 17 Oct), per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

## In flight

#33 (061 + 061a) and #34 (pnpm pin) **merged by Sunny on 17 Oct, about 22:00 IST**, both with merge commits (Sunny's report; the next chat verifies on GitHub).

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

- [P004](decisions/P004-target-reset.md): pitch the honest benchmark and the live product rather than "beats Greptile". Proposed, not agreed.
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
