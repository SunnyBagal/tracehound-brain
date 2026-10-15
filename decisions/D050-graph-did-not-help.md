---
id: D050
title: Held-out results — the graph did not help the repair agent
date: 2026-10-06
kind: experiment
lane: benchmark
outcome: loss
call: sound
judged_by: chat
judged_on: 2026-10-15
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#050--held-out-results-imported-the-graph-did-not-help-the-repair-agent-on-recall
pr: 14
---

# D050 · The graph did not help the repair agent

## Decided
Import the held-out evaluation (tag `eval-freeze-2`, 160 counted runs) into the repo, with every table recomputed from the run records. Say plainly in the README that the graph does not help.

## Why, at the time
The protocol was frozen before the runs, so the result had to be reported whichever way it fell.

## Expected
That graph-on would beat graph-off. That was the project's premise, not a written threshold.

## What happened
- Resolved, graph-on vs graph-off (n = 40 each): Nano 11 vs 13, Super 28 vs 26.
- Verified at stop: Nano 11 vs 19, Super 30 vs 31.
- **Graph tool calls: 0 in 80 graph-on runs.** The agent never used the graph it was offered.

## Verdict
- **Outcome:** loss for the hypothesis. Reporting it honestly was a win for credibility.
- **Call:** sound. Freezing the protocol first is what made the loss believable.
- **What we'd do differently:** check tool-use rates on a small dev batch before a full held-out run. 0 of 80 calls was knowable early.

## Links
- Led to [P001](P001-pivot-to-reviewer.md). Cited again in [D057](D057-repo-context.md) as the reason not to let the model search the repo with tools.
