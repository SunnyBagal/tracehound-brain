---
updated: 2026-10-16 19:15 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `c70e931` (merge of #29, 059).
- **Open, checked by chat 2026-10-16 19:10 IST:**
  - **#30** Codex: Action switched to pool-verify-v1, head `813f22f`. Diff checked; CI not visible to the chat, so look at the Checks tab.
  - **#31** relay 060 scorer checks, head `89c77fd`, freeze `b98f014`, CI green. Report only.
  - The two touch disjoint files. Merge both.
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius ledger: $12.45236 of $45. `nebiusCapUsd` = 5 (set for 060).

## In flight

| Lane | Job | Status |
|---|---|---|
| Relay | 060 done (report only) | Waiting for #31 to merge and the next brief (Sunny to choose: freeze now, or one more verifier round) |
| Codex | #30 done | Next: the demo repo (repo choice open) |
| Grok | R1–R3 filed ([research](research/README.md)) | All 15 checked citations real |

## Known problems

- **Codex automatic review** has posted nothing on #27–#31. Check the Review trigger, or comment `@codex review`.
- **No live reviewer demo yet**, and judges may score from the video and text alone (R3). The video and README matter as much as the demo.

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 done (adopted) | #28 merged |
| 17–18 Oct | 060 scorer fairness: augmented precision plus a second judge | Demo repo with the action installed and real review comments (the judges' working demo); switch the action to the reviewer that wins |
| 19–20 Oct | Spare, or a second precision round | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the one Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md, "what changed since 26 August" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct (deadline 30 Oct 22:30 IST) | |

## Open decisions

- [P004](decisions/P004-target-reset.md): pitch the honest benchmark and the live product rather than "beats Greptile". Proposed, not agreed.
- Decided 2026-10-16 (Sunny): (a) first, as 059 pool-verify-v1. Next after it: 060, augmented precision for every reviewer plus a second judge.
- Decided 2026-10-16 (Sunny): the Action switches to pool-verify-v1; the 060 second judge is Qwen3-235B-A22B-Instruct-2507; 060 covers the second judge plus augmented precision.
- Demo repo: which repo hosts the demo PRs? Open.
- Cal.com run: pool-verify-v1 primary. Chat recommends declaring diff-v1 and pool-raw as secondary rows in advance; they come free from the same pipeline.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30, and decisions 001–031 first appear that same day, so they were likely carried over from earlier work. Is the earlier history (FixFlow) somewhere public? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.
