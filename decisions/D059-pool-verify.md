---
id: D059
title: Pooled candidates filtered by the context verifier, pool-verify-v1
date: 2026-10-16
kind: experiment
lane: benchmark
outcome: win
call: sound
judged_by: chat
judged_on: 2026-10-16
repo_entry: pending merge of PR #29
pr: 29
---

# D059 · pool-verify-v1

## Decided
Pool review-diff-v1's findings with review-ctx-v1's findings that diff-v1 didn't already make (same path within 3 lines counts as already made). Filter the pool with verify-ctx-v1, unchanged. diff-v1 findings reuse their 058 verdicts, so only the ctx-only findings get new verifier calls. No new prompt and no new reviewer run. Chosen by Sunny on 2026-10-16 over doing the scorer-fairness job first.

## Why, at the time
- 058 showed the verifier separates true from false findings (it kept 69% of TPs and 33% of FPs), but diff-v1 alone supplies only 32 TPs.
- ctx-v1 finds some defects diff-v1 misses, so a wider pool should raise recall after filtering.
- It costs well under $1 and reuses frozen pieces.

## Expected (pre-registered in the brief, before any run)
- Adopt only if F ≥ 35.0% on the 40 dev PRs. Otherwise diff-v1 stays the default.
- All 143 reused 058 verdicts must match their findings, or the job stops.
- Freeze commit (rules and pool builder) pushed before the first verifier call. Nothing under `packages/analyzer/src/review/` changes.
- Known bias: this is the fourth variant tested on the same dev set, so whichever wins is favoured by selection. Only the Cal.com run confirms it.

## What happened (n = 74, one run, one judge)
| run | comments | P | R | F |
|---|---|---|---|---|
| review-diff-v1 | 143 | 25.6% (32/125) | 43.2% | 32.2% |
| pool-raw (before filtering) | 243 | 24.5% (38/155) | **51.4% (38/74)** | 33.2% |
| **pool-verify-v1** | 83 | 36.1% (26/72) | 35.1% (26/74) | **35.6%** |

- Freeze `8c78a75` at 10:02:05Z; first verifier call 10:02:26Z. After the freeze, only a new scorer script was added. Reuse check: 143/143.
- Pool: 143 diff-v1 findings, plus 100 ctx-only (33 agreed and dropped). The verifier kept 20 of the 100 ctx-only findings (only 5 of discourse's 70).
- **pool-raw's recall (51.4%) is above every commercial tool except Augment (55.4%).** The candidates exist; the verifier loses 12 of the 38 real ones.
- Per repo, pool-verify-v1 is not the best TraceHound row anywhere. The win is in the aggregate.
- Margin: one TP judged as FP would give F 34.2%, below the bar.
- Product cost (diff + ctx + verifier): median $0.0317 per PR, max $0.2308. Wall time median 100 s, max 581 s, run in sequence.
- Job spend $0.21517; ledger $11.81728.

## Verdict
- **Outcome:** win by the pre-set rule (35.6 ≥ 35.0). Adopted as the configuration for the freeze and the Cal.com run. The first variant to clear a bar.
- **Call:** sound. Cheap, reused frozen pieces, rules frozen first. But the margin is within judge noise, and this is the fourth variant tried on the same dev set, so selection favours it. Only Cal.com confirms it.
- **What next:** the verifier is now the bottleneck on recall: it keeps 26 of 38 TPs. The scorer checks (060) matter more than another reviewer tweak.

## Links
- Brief: `~/Projects/tracehound-ops/briefs/059-pool-verify.md` (saved in the repo as `docs/prompts/059-pool-verify.md` by the job). Previous: [D058](D058-verify-ctx.md).
