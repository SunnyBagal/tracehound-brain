---
id: P002
title: Lanes — relay for the benchmark, Codex for the product, Grok for audits
date: 2026-10-14
kind: process
lane: chat
outcome: pending
call: unclear
judged_by: chat
judged_on: 2026-10-15
repo_entry: none (AGENTS.md lists the lanes)
pr: 25
---

# P002 · Lanes and tools

## Decided
| Who | Job |
|---|---|
| Chat (Claude, claude.ai) | Plans, writes every prompt, checks every PR from GitHub before Sunny merges |
| Relay (Claude Code builder + reviewer) | Benchmark lane, one job at a time, `~/Projects/TraceHound` |
| Codex (ChatGPT Plus) | Product lane on separate paths (`.github/actions/`, `packages/action/`, `docs/action/`, `docs/demo/`), worktree `~/Projects/TraceHound-codex`; also automatic PR review |
| Grok (Premium) | No code. Audits each results doc after merge, X posts, video script |

## Why, at the time
Two lanes running in parallel, on paths that don't overlap, roughly double throughput with 14 days left. Using a different model for review and audit catches what Claude misses.

## Expected
Not recorded.

## What happened (so far)
- Codex shipped the GitHub Action (#25). CI was green, all files in its lane.
- Codex's review found a real but minor race (stale head before posting).
- Grok passed its first kill test ([P003](P003-grok-kill-test.md)).
- One cross-lane snag: the AGENTS.md / CLAUDE.md copy in relay's checkout ([lessons](../lessons.md)).

## Verdict
- **Outcome:** pending
- **Call:** unclear. It has worked for one day. Judge after the demo repo task.
