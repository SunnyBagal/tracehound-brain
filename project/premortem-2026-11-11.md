---
title: Premortem of the held-out run
date: 2026-11-11
source: https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/premortem.md
---

# Premortem of the held-out run (2026-11-11)

A planner session listed, before anything ran, the ways the held-out comparison could end up voided or disputed ([docs/plan/premortem.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/premortem.md)). $0, nothing executed, no held-out file opened.

## Counts
- **22 risks**, ranked by the planner's own estimate; **6** marked likely. Each has its evidence, a $0 check and the lane that runs it.
- **20 gaps** in the plan, specs and runbooks against that list; on 2026-11-12 the other planner closed 15 of the 17 it owned in its documents.
- **3 gaps needed the owner's ruling**, all ruled in [decision 076c](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076c--planner-bs-protocol-review-premortem-and-team-process-ruled-2026-11-12): what counts as a completed review with no comments, measuring whether the hand-check is blind (a guess column), and the smallest paired sample still tested (15 of 20).

## What changed because of it
- The trigger script gets a re-trigger mode (it could not post the one allowed re-trigger), and the hourly triggers move to a detached scheduler with a rehearsal.
- The frozen run's checkout uses the main ledger and environment, so the hard spending stop sees all spend.
- A second agent re-derives the freeze hashes and the final score independently, and counts whether any case branch could contain another case's fix.
- From the window's start, the agents that tune the reviewer do not read the forks' pull requests, issues or notifications.

## Links
- [NOW](../NOW.md), [plan audit](plan-2026-11-10-audit.md), [timeline](../timeline.md).
