---
updated: 2026-10-16 15:45 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `c34b2ed` (merge of #28: action fixes, Codex review rules, lockfile fix). #27 (058) merged at `7a92b88`. No open PRs.
- Recall main `20ac149`, `testable-baseline` `57d920e`. Untouched since 2026-10-07. Never open Recall PRs #1 or #2.
- Nebius ledger: $11.60211 of the $45 total cap. Per-run cap `nebiusCapUsd` = 3. See [costs](ops/costs.md).

## In flight

| Lane | Job | Status |
|---|---|---|
| Relay (benchmark) | 059 pool-verify-v1, brief `briefs/059-pool-verify.md` ([D059](decisions/D059-pool-verify.md)) | Started 2026-10-16 ~15:30 IST. Expect a freeze commit, then a PR with STOP AND REPORT as a comment |
| Codex (product) | Next: the demo repo | Prompt not written yet |

## Known problems

- **Codex automatic review** posted nothing on #27 or #28. Check its Review trigger setting, or comment `@codex review`.
- **No live reviewer demo yet.** This is the top hackathon risk ([hackathon](project/hackathon.md)).

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 pool-verify-v1 (running) | #28 merged |
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
- Product reviewer for the demo: diff-v1 (P 25.6%) is what the action runs today. agree-diff-ctx has P 57.7% but needs two reviewer runs per PR. Decide after the next job.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30, and decisions 001–031 first appear that same day, so they were likely carried over from earlier work. Is the earlier history (FixFlow) somewhere public? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.
