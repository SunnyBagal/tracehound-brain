# R17 · Multi-agent research review: what to do next (2026-11-07)

> **Check note (bunny, backfill, 2026-11-11).** This is not a Grok report. Five agents ran it and checked each other: the run's own [VERIFY.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/research/2026-11-07/VERIFY.md) records 24 spot-checks. ag failed 4 of its checks; those claims were dropped or corrected in the report.
> I re-checked 4 of the report's repo citations, and all 4 hold:
> - `levers-v2.md:45-46`: kept precision 54.7 / 54.8%;
> - `budget.ts:28`: `class Budget` starts there;
> - `config/prices.json:9-10`: Ultra at $0 and Lightning a placeholder;
> - `decisions.md:3449-3450` at the report's HEAD `b498647`: no further Cal.com run is allowed.
>
> I opened no external URL.

Source: [docs/research/2026-11-07/REPORT.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/research/2026-11-07/REPORT.md) (committed `53b6045`).

## Question
After the hackathon: is TraceHound's reviewer worth continuing, what would "beating" a tool mean, and what is the single next task?

## Method
- **Agents:** five: claude (orchestrator), codex, devin, Grok ("scout") and Antigravity ("ag"), each with a brief and its own output file.
- **Process:** two rounds of questions through files, two votes on a draft conclusion, and spot-checks of every agent's claims.
- **Limits:** research only. No code edits, no commits, 0 Nebius calls; the only network read was the free public `models_info` GET.
- **Blindness:** no Cal.com golden or tool comment was opened. Dev per-comment score files reached the workers as counts only.

## Findings
- **What "beating" means:** a 95% PR-bootstrap interval of ΔF that excludes 0, on a held-out set, under a matcher fixed in advance. A point-estimate rank never counts.
- **The gap today:**
  - all 50 PRs: behind Augment by 14.0 F points (interval-backed); within noise of Cursor and Greptile;
  - Cal.com: spent.
- **Ahead of Augment is not supported by evidence.** It would need about +14 F, and the best lever measured (the 3.5 swap) gave +8.1 [+1.4, +14.4].
- **Verdict:** continue, with a narrower goal: reliable output, a fresh TypeScript held-out set, and one context lever tested on it.
- **Cost:** about $0.063 median per PR. The two reviewers' long reasoning outputs are the cost, and Token Factory ignores reasoning budgets ([D070](../decisions/D070-recall-v3.md)).
- **Agent context files:** which tools auto-load which files (AGENTS.md, CLAUDE.md, `.agents/`). Proposal: a short `AGENTS.md`, `CLAUDE.md` without its long state block, and `.agents/STATE.md` (≤ 60 lines, rewritten) plus `.agents/LOG.md` (append-only).

## The next task it chose
A pre-registered dev A/B of the two generation-recovery switches already built in #67, capped at $4.
- **Vote:** 2–2 in the second round.
- **Dissent:** devin's dissent on the precision gate (E3) was recorded, and the owner later adopted devin's version.
- **The ranked plan after it:**
  1. observability;
  2. a 2-repo SZZ label pilot;
  3. a fresh TS dev + held-out set;
  4. symbol-resolved context on the TS dev set;
  5. a Lightning verifier.

## What it changed
- Decision 073 ran the recovery A/B the next day ([D073](../decisions/D073-recovery-ab.md)), then 074 and 075.
- The label pilot and the TS set became Track B ([track-b-testset](../benchmark/track-b-testset.md)).
- `.agents/STATE.md` and `.agents/LOG.md` were created on 2026-11-09, and the multi-agent setup became the standing team ([P010](../decisions/P010-herdr-team.md)).
