---
id: D058
title: Context-aware verifier, verify-ctx-v1
date: 2026-10-15
kind: experiment
lane: benchmark
outcome: loss
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: pending merge of PR #27
pr: 27
---

# D058 · verify-ctx-v1

## Decided
Keep review-diff-v1 as the generator, since it has the recall. For each of its 143 findings, a Super verifier sees per-finding repository context and answers keep or drop. The per-finding context is the finding's file window, its hunks, and references to the enclosing function, within 24,000 characters. Also an agreement table, reported only: diff-v1 findings that ctx-v1 also flagged within 3 lines.

## Why, at the time
- 057: context makes Super reject bad findings (FP 93 → 37) but also stops it producing good ones (TP 32 → 21). So use context to judge, not to generate.
- 056: confidence numbers didn't separate TP from FP, so ask for a decision instead.

## Expected (pre-registered, in freeze commit `c2f08be`)
- A finding is kept if and only if the verdict is keep. An invalid reply after one retry is kept and counted as VERIFY_FAILED.
- Adopt only if F ≥ 35.0% on the 40 dev PRs.
- Freeze commit pushed before the first verifier call. It was pushed at 15:03:54Z; the first call came at 15:04:09Z, and the prompt and code are unchanged after it.

## What happened (n = 74, one run, one judge)
| run | P | R | F |
|---|---|---|---|
| review-diff-v1 | 25.6% (32/125) | 43.2% | 32.2% |
| verify-ctx-v1 | 38.9% (21/54) | 28.4% (21/74) | 32.8% |
| agree-diff-ctx (report only) | 57.7% (15/26) | 20.3% (15/74) | 30.0% |

- The verifier **does separate**. It kept 22 of 32 TPs (69%) but only 31 of 93 FPs (33%). Compare 056, where 6 of 32 TPs got confidence 1.
- It still cost 10 of 32 TPs, so F barely moved.
- 63 of 143 kept, 0 VERIFY_FAILED, 3 retries. References reached 97 of 143 findings (median 6).
- Cost $0.33 (ledger $11.60211).

## Verdict
- **Outcome:** loss by the pre-set rule (32.8% < 35.0%). Not adopted.
- **Call:** sound. It applied both lessons from 056 and 057, and it showed for the first time that context can tell true findings from false ones. The bottleneck is now recall, not filtering.
- **What we'd do differently:** nothing in the method. The next lever is the candidate pool. Generate wider (for example the union of runs), then filter with this verifier.

## Links
- PR #27; `docs/bench/ctx-verify.md`; [results](../results.md)
