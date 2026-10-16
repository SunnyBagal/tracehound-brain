---
updated: 2026-10-16 17:45 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `c34b2ed` (merge of #28). **Open: #29** (059, head `0fe0019`, freeze `8c78a75`, CI green, checked by chat 2026-10-16 17:15 IST). Ready to merge.
- Recall main `20ac149`, `testable-baseline` `57d920e`. Untouched since 2026-10-07. Never open Recall PRs #1 or #2.
- Nebius ledger: $11.81728 of the $45 total cap. Per-run cap `nebiusCapUsd` = 3. See [costs](ops/costs.md).

## In flight

| Lane | Job | Status |
|---|---|---|
| Relay (benchmark) | **060 scorer checks**, brief `briefs/060-scorer-checks.md` ([D060](decisions/D060-scorer-checks.md)): second judge Qwen3-235B, plus augmented precision | Written 2026-10-16. Start after #29 merges **and `nebiusCapUsd` is set to 5** |
| Codex (product) | `codex-03-action-pool-verify.md`: switch the Action to pool-verify-v1 (diff and ctx reviewers in parallel, pool, verify) | Written 2026-10-16. Start after #29 merges (it imports `scripts/bench/agree-rule.ts`). Next: the demo repo (repo choice open) |

| Grok | Research R1–R3 ([research](research/README.md)): verifier methods, market and benchmarks, hackathon and demo | Sent 2026-10-16; paste the answers to the chat for a citation check |

## Known problems

- **Codex automatic review** posted nothing on #27, #28 or #29. Check its Review trigger setting, or comment `@codex review`.
- **No live reviewer demo yet.** This is the top hackathon risk ([hackathon](project/hackathon.md)).
- **Same model reviews and judges** (Super); 060 adds a judge from a different family.

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
