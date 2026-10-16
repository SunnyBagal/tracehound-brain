# Phase 4: the PR reviewer (decisions 054–059, 7 Oct onwards)

**Pivot on 7 Oct** ([P001](../decisions/P001-pivot-to-reviewer.md)): TraceHound becomes a PR reviewer, measured against five commercial tools on a public benchmark. Plan: `docs/reviewer-plan.md`.

## The experiments, in order

Each step names the problem it attacked. Dev set: 40 PRs, n = 74. Numbers are P / R / F.

| # | What | Result | Verdict | Problem it left |
|---|---|---|---|---|
| 054 | Scorer: one judge (Super) scores everyone | Reproduces the published tool ranking; our judge is kinder on precision | win | Golden set favours the recorded tools; one judge |
| 055 | review-diff-v1: diff only | 25.6 / 43.2 / **32.2** | baseline | 3 of 4 comments wrong |
| 056 | verify-v1: second pass, diff only, confidence 1–5 | The pre-set rule kept everything; confidence didn't separate TP/FP | loss, flawed | Verifier knew nothing new |
| 057 | review-ctx-v1: reviewer sees repo context | 36.2 / 28.4 / 31.8 | tie, flawed brief | Context makes the generator timid; references mostly cut by the budget |
| 058 | verify-ctx-v1: context used only to judge | 38.9 / 28.4 / **32.8**; kept 22/32 TPs, 31/93 FPs | loss by bar (35.0), sound | Separates well, but the pool has too few TPs |
| 058 | agree-diff-ctx (report only) | 57.7 / 20.3 / 30.0 | — | Highest precision, lowest recall |
| 059 | pool-verify-v1: diff-v1 ∪ ctx-only → verify-ctx-v1 | running | — | — |

Product side, in parallel (Codex): GitHub Action (#25), head recheck, example workflow `@main`, Codex review rules in AGENTS.md, lockfile fix (#28).

## What we learned (the through-line)

1. **Precision is the gap; recall is fine.** Diff-only recall matches CodeRabbit.
2. **Information has to be new to help.** A verifier seeing the same diff (056) can't judge. One seeing context (058) can.
3. **Where context goes matters.** In the generator, it suppresses findings (057). In the judge, it filters them (058).
4. **Models don't use optional tools** (phase 2), so context is always injected deterministically.
5. **Process:** pre-registered bars plus freeze commits make every loss credible. A budget must never cut the variable under test.

Scoreboard: [results](../results.md). Tools: [competitors](../benchmark/competitors.md).
