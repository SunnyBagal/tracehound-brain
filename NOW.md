---
updated: 2026-10-17 16:10 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `f437ecb` (#30 Action on pool-verify-v1 and #31 060 merged 2026-10-16/17). No open PRs.
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $46.22** (2026-10-17); ledger $12.45236. Caps: total → $52 in .env, per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

## In flight

| Lane | Job | Status |
|---|---|---|
| Relay | **061 test plan** (`briefs/061-test-plan.md`): pre-register the Cal.com run and write the run code, no model calls | Written 2026-10-16. Revised 2026-10-17 to add repeat runs (spread). #31 is merged, so it can start now. Then 062 executes the plan (cap $15) |
| Codex | **codex-04** dogfood + demo: TraceHound reviews its own PRs; `SunnyBagal/tracehound-demo` (Recall @ `57d920e`) with the 4 seeded dev bugs plus 1 control PR | Written 2026-10-16. Needs: #30 merged, `NEBIUS_API_KEY` secret in both repos, empty demo repo created |
| Grok | R1–R3 filed ([research](research/README.md)) | All 15 checked citations real |

| Codex queue | **codex-05** fix pnpm setup → re-run 5 demo PRs + dogfood #32; then **codex-06** fix suggestions + review-data footer | codex-05 sent 2026-10-17 |
| UI lane (P006) | **ui-01** viewer round 1 (no dev clutter, fresh snapshots, expandable nodes, inline code) in ~/Projects/TraceHound-ui, plain Claude Code session; then ui-02 dashboard + PR overlay + benchmark page | ui-01 + addendum (kind colours, Super one-line summaries) sent 2026-10-17 |

## Known problems

- **Codex automatic review** has posted nothing on #27–#31. Check the Review trigger, or comment `@codex review`.
- **All 7 Action runs failed (17 Oct):** pnpm setup can't find a version in other repos. Fix: codex-05 (`version: 11.20.0`).
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
