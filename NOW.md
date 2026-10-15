---
updated: 2026-10-15 21:20 IST
updated_by: chat
---

# Now

Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

## Repo state

- TraceHound main: `5ee7677` (merge of #26, decision 057). #25 (GitHub Action) merged at `a38148b`.
- Open, both checked by chat 2026-10-15 21:20 IST:
  - **#27** relay 058 (`e3790b0`, freeze `c2f08be`). CI green (run 38758282829). Ready to merge; it records a loss.
  - **#28** Codex action fixes (`6ec9a3b`). The diff is correct; CI was not visible from the chat. Before merging, add the lockfile fix (below).
- Recall main `20ac149`, `testable-baseline` `57d920e`. Untouched since 2026-10-07. Never open Recall PRs #1 or #2.
- Nebius ledger: $11.60211 of the $45 total cap. Per-run cap `nebiusCapUsd` = 3 (`~/Projects/tracehound-ops/ops/lanes.json` line 11).

## In flight

Nothing running. Relay waits for #27 to merge and for the next brief.

## Known problems

- **Lockfile drift:** main's `pnpm-lock.yaml` has no importer for `packages/action`, so every local `pnpm install` rewrites it and dirties the tree. That is a step-0 trap for the next relay run. Fix: one commit adding only that importer (on #28's branch).
- **Codex automatic review** posted nothing on #27 or #28. Check its Review trigger setting, or comment `@codex review`.

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | Next job (a) or (b), see Open decisions (058 finished early) | Merge #28 with the lockfile fix |
| 17–18 Oct | The other of (a) and (b) | Demo repo with the action installed and real review comments (the judges' working demo); switch the action to the reviewer that wins |
| 19–20 Oct | Spare, or a second precision round | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the one Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md, "what changed since 26 August" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct (deadline 30 Oct 22:30 IST) | |

## Open decisions

- [P004](decisions/P004-target-reset.md): pitch the honest benchmark and the live product rather than "beats Greptile". Proposed, not agreed.
- Next benchmark job, two candidates: (a) a wider candidate pool (union of diff-v1 and ctx-v1 findings) filtered by verify-ctx-v1 unchanged, which 058 suggests could clear F 35%; (b) 059, augmented precision for every reviewer plus a second judge. The chat recommends (a) first, since it is cheap, then (b).
- Product reviewer for the demo: diff-v1 (P 25.6%) is what the action runs today. agree-diff-ctx has P 57.7% but needs two reviewer runs per PR. Decide after the next job.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30. Does earlier history exist elsewhere (FixFlow), or does everything count as new? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.
