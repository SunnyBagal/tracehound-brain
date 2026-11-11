---
id: P010
title: A Herdr multi-agent team with fixed lanes and file handoffs
date: 2026-11-08
kind: process
lane: chat
outcome: pending
call: too-early
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: none (CLAUDE.md "Herdr team"; docs/herdr.md)
pr: none (CLAUDE.md commit 82c1f08)
---

# P010 · Herdr team

## Decided
From 2026-11-08, work runs as a team of CLI agents in one Herdr workspace. Each agent has a lane (CLAUDE.md, "Herdr team"):

| who | lane |
|---|---|
| artu1 (main Claude) | plans, pre-registers decisions, assigns lanes, merges, writes final reports; builds only when no builder is free |
| bunny (Claude, second account) | main builder: implementation on branches |
| codex | code review, GO/NO-GO before any paid run or merge; second builder |
| devin | methodology review (pre-registrations, gates, cache and blindness rules, what a result means); independent labelling |
| grok | web and docs research with source URLs; leak and consistency audits (artu1 spot-checks its claims) |

## How it works
Commands: [docs/herdr.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/herdr.md), herdr-core `hc`.
- **Addresses:** `hc ls` lists terminals and their t-addresses. Names are not addressable in this build.
- **Sending work:** `hc send <t> "<one line>"` pastes a prompt that points at a brief file.
- **Waiting:** `hc wait <t>` waits for the turn to end.
- **Reading a screen:** `hc read <t>` reads a screen, but you act on result files, not panes.
- **`.done` handoff:**
  1. artu1 writes `run/briefs/<agent>-<task>.md`;
  2. the worker writes `run/out/<agent>.<task>.md`, then a `.done` marker **last**;
  3. no marker means not finished, whatever the pane says.

  Grok can show "idle" while still working, so only its `.done` file counts.
- **Questions:** questions between agents go through `run/comms/` files, with at most 2 per step and a 10-minute wait.
- **Rules:**
  - only artu1 approves spend;
  - no agent answers another's permission dialog;
  - each worker writes only its own paths.
- **State:** `.agents/STATE.md` (≤ 60 lines, rewritten each session) and `.agents/LOG.md` (append-only, one entry per day) hold state across sessions.

## The firewall (general terms)
Agents who tune TraceHound must stay blind to held-out test data. They never open held-out files, and never read the screen of an agent doing held-out work. Held-out tasks go to the other agents, and only numbers-only reports cross over. See [track-b-testset](../benchmark/track-b-testset.md).

## Why, at the time
- The [7 Nov research run](../research/R17-multi-agent-review-2026-11-07.md) showed that five agents checking each other caught errors in each other's claims, the orchestrator's included.
- Separate builder and reviewer lanes replaced the hackathon's chat → relay setup ([P002](P002-lanes-and-tools.md)).

## Expected
Not recorded.

## What happened (so far)
- **073–075:** each paid job ran only after a codex GO, and some took several review rounds (074: three).
- **Builder work:** the local runner went NO-GO → NO-GO → GO before merge ([local runner](../ops/local-runner.md)).
- **Track B:** labelling ran in parallel with blind second labels.
- **Quota limits:** devin hit its daily, then weekly, quota (out from 2026-11-10).
- **Firewall slips:** two, both disclosed ([lessons](../lessons.md)).

## Verdict
- **Outcome:** pending.
- **Call:** too-early. Judge after the held-out run.

## Links
[P002](P002-lanes-and-tools.md) · [R17](../research/R17-multi-agent-review-2026-11-07.md) · [local runner](../ops/local-runner.md)
