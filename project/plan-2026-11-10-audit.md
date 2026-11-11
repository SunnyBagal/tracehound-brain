---
title: Audit of the plan to the held-out run
date: 2026-11-11
source: https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/2026-11-10-plan-audit.md
---

# Audit of the plan to the held-out run (2026-11-11)

The plan ([docs/plan/2026-11-10-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/2026-11-10-plan.md), written by a planner session) was audited before any of it ran, under Sunny's new rule: **plans are hypotheses; no task runs unless its premises check out against our own data.** $0, nothing executed, the plan not edited. The merged audit is in the repo: [docs/plan/2026-11-10-plan-audit.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/2026-11-10-plan-audit.md).

## How
Three independent lanes, merged by artu1:
- codex: internal claims (code, ledger, decisions, docs), each with file:line;
- bunny: statistics and budget arithmetic, with working;
- grok: external claims (vendor docs), with URLs; artu1 re-found the quoted phrases on the cited pages (8 of 8).

## Verdict counts
| lane | claims | verified | contradicted | unsupported | marked guesses |
|---|---|---|---|---|---|
| codex | 131 | 105 | 10 | 10 | 6 |
| bunny | 23 | 13 | 6 | 4 | — |
| grok | 31 | 23 | 1 | 7 | — |
| **total** | **185** | **141** | **17** | **21** | **6** |

The planner accepted all six of bunny's contradictions.

## Plan-level findings
- **Power:** with 20 held-out bugs, a true 30-point recall gap between two tools is detected only about 4 times in 10 (power 0.37–0.39 for the example pairs 20% vs 50% and 15% vs 45%; exact paired sign test, α 0.05, tools catching bugs independently). Positively correlated catches raise it (0.44–0.53 for 20% vs 50% at correlations 0.2–0.4), and that correlation is unmeasured. Under the same model a coin-flip chance needs about 35 points and 80% power about 48; "no detectable difference" is the expected honest headline, and "catches as many as" is not a supportable claim.
- **Caps:** the local runner starts a case only if the spend so far plus the per-case cap fits the run cap, so the plan's multi-batch run caps stop early and no command enforced them; caps must be set per invocation with headroom. The smoke-test cap was below its own per-case cap.
- **Costs:** the optional context step was costed at about $1.55 but is nearer $2.17; "about $0.08 per PR" matched neither the measured $0.075 (non-TypeScript) nor the plan's own uplift. Several cost bases are historical averages, not TypeScript measurements.
- **Gates:** the arm-selection, redraw and non-regression gates have error rates that depend on quantities not yet measured on TypeScript; one argument (that one arm's comments include another's at the 15-comment cap) is false.
- **Rules:** the plan's tie rule disagreed with the budget's (simpler arm wins); a counting rule for tool comments already existed; CodeRabbit's limit is per developer and per repository, not per fork.
- **Money:** the $3 reserve is inside the $17.25 allocation; with $1.06 unallocated and the $1.00 margin that is the whole $19.31.

## What it changed
13 of the plan's tasks are marked **blocked** until a correction or a $0 measurement: A1 (budget split and gates), D1 (protocol), B2 (part of the scoring kit), S1 (smoke test), P1 (dev PRs), R1 and R2 (dev runs), E1 (the lever tests, except the $0 context check), L4 (report-only union), A2 (choosing the candidate), F1 (freeze), H1 (held-out window), W1 (write-up). Clear: Sunny's rulings, lineage, the $0 dev audits, the code review, the tool-facts research, scoring. The corrections are listed for Sunny in the repo audit; the plan itself was not edited.

## Links
- Rule: TraceHound [CLAUDE.md](https://github.com/SunnyBagal/TraceHound/blob/main/CLAUDE.md) ("Plans from any model are hypotheses").
- [NOW](../NOW.md), [timeline](../timeline.md), [ledger vs billing](../ops/ledger-vs-billing.md), [local runner](../ops/local-runner.md).
