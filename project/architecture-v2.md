---
updated: 2026-10-19 01:30 IST
updated_by: chat
status: proposed (Sunny to approve)
---

# TraceHound v2 architecture (19 → 29 Oct)

## 1. Where we are, honestly

The gap to the top two is **precision**, not recall. Across all 50 PRs, we post 57 wrong comments; Augment posts 16 and Cursor posts 17. Our recall (41.2) is close to Cursor's (43.3), and our raw pool reaches 51–70% recall before filtering.

| Focus area | What runs today | Gap | Evidence |
|---|---|---|---|
| Noise and nitpicks | verify-ctx-v1 keep/drop; cap of 15; no file filter; no category; no severity | The worst gap. P 41.2 vs Augment 77.8 / Cursor 71.2 | results.md, all-50 table |
| Cross-file context | `git grep -w` references of changed names + changed files (ctx-v1, verify-ctx-v1). The TS graph (extractors, `changes`, `impact`) is **not used by the reviewer** | Context makes the model more conservative, not sharper (D057). Nothing looks for breaks in files the PR **didn't** edit | D057, D058, D050 |
| Actionability | 1–4 sentence body; committable fix suggestions (#38) | No scenario, no evidence citations, no severity | prompt.ts output schema |
| Adaptive learning | none | No conventions input, no memory of rejected comments | — |
| Ingestion | GitHub Action on PR events + push; data branch (#43) | Enough for the hackathon | #43 |
| IDE | none (an MCP server exists for graph queries only) | — | agent/mcp.ts |

**What "beating Augment and Cursor" means in 10 days.** All-50 top 2 means F above Cursor's 53.8. With recall held at about 45, that needs precision of about 65. That's plausible if gating works, and it's not guaranteed. Beating Augment (66.3, P 77.8) by 29 Oct is unlikely; we won't plan around it. Where we can honestly be **first**: cross-file contract breaks on a pre-registered seeded test, cost per PR, and an open model and code.

## 2. The pipeline

```
 PR event (Action)                                     [exists]
   │
 S1 Pre-filter (deterministic)                         [new: codex-08]
   │  drop lockfiles, generated, vendored, minified, binary, snapshots, whitespace-only hunks
   │
 S2 Repo context                                        
   │  a. changed files + git grep refs                  [exists: ctx-v1]
   │  b. contract map: what the PR changed that other   [new: cc-03]
   │     files depend on — typed (tsc base/head diff) and
   │     string-keyed (routes, queue names, Redis keys, env vars, events)
   │
 S3 Generators
   │  diff-v1 (Super)                                   [exists]
   │  ctx-v1 (Super)                                    [exists]
   │  contract findings, deterministic (no LLM)         [new: cc-03]
   │  contract-v1 specialist (Super, sees contract map) [later: relay, only if S2b finds dependents]
   │
 S4 Agreement vote across N samples                     [DROPPED: 064 lost; P up, R down, 5× cost]
   │
 S5 Evidence verifier v2 ("prove it or drop it")        [new: relay 065]
   │  must return: verdict, ≥1 exact quote with path:line, a concrete failure
   │  scenario, severity, category
   │
 S6 Post-gate (deterministic)                           [new: codex-08]
   │  quote must exist verbatim at the cited line; line must be in the diff
   │  (contract findings exempt); drop style/nit categories and patterns;
   │  drop suppressed fingerprints (S8); dedupe; severity floor; per-PR cap;
   │  silence is allowed. Every drop is logged with a rule id.
   │
 S7 Deliver                                             [exists + format change in integration]
   │  body: What breaks / When (scenario) / Evidence (links) / Fix (committable)
   │  summary: "N files you didn't edit depend on X"; drop log counts
   │  data branch write (#43)
   │
 S8 Learn                                               [new: codex-09]
      conventions: .tracehound/rules.md + AGENTS.md / CLAUDE.md / CONTRIBUTING excerpts → S3, S5
      feedback: 👎 reactions, threads resolved without a change, "won't fix"/"nit" replies →
      fingerprints in tracehound-data/feedback/ → S6 suppression
```

Focus area → stages: **noise** S1, S4, S5, S6; **cross-file** S2b, S3 contracts; **actionability** S5 and S7; **learning** S8.

### Stage contract (so parallel builders fit together)
Every stage is a pure function with a JSON-in/JSON-out CLI subcommand in `packages/analyzer/src/bin.ts`. A candidate is structurally:

```ts
{ path: string; line: number | null; body: string;
  source?: "diff" | "ctx" | "contract" | "tsc";
  votes?: number;                                       // S4
  evidence?: { path: string; line: number; quote: string }[];  // S5
  scenario?: string; severity?: "high" | "medium" | "low";
  category?: "correctness" | "contract" | "security" | "performance" | "style" }
```

Each lane declares this type locally. The integration job unifies them into `packages/analyzer/src/candidate.ts`.

### What we deliberately don't build
- **A webhook server, a queue or a GitHub App.** The Action already gets every event. A service is ops work that judges can't see and that would eat days.
- **A swarm of generic specialists** (security, performance, style…). Our raw recall is already high, and every extra generator adds false positives. The only new specialist is the contract one, because it sees something the others can't.
- **An IDE plugin.** At most `tracehound review --local` plus one MCP tool, after the freeze, if there's a spare day.
- **Embeddings for S8.** Lexical fingerprints are deterministic, testable and good enough for a demo.

## 3. Measurement (where selection bias could fool us)
- **Deterministic stages are evaluated for $0 on recorded dev outputs** (S1, S6 and the S4 vote need no model calls). Their rules are written before anyone looks at the dev scores, and builders never see bench data.
- S5 is a new prompt, so it gets a pre-registered dev test (relay 065).
- **Don't run 065 alone.** Every Cal.com run is another look at the test set. Do one look at the full v2 pipeline after freeze-2.
- **Freeze-2: 24 Oct.** Then exactly one blind Cal.com run of v2 (stated as the second look at that set), plus the all-50 table under both judges.
- **Seeded cross-file test** on Linea and Recall (cc-04, below), pre-registered, with seeds written by a session that never sees the cc-03 code. It includes clean controls and compile-clean semantic breaks, so tsc alone can't ace it. Where free tiers allow it, competitors run on the same PRs (grok-07 finds out which ones can).
- ReviewBench 60 (R7) stays optional; it needs Anthropic API credit for the judge.
- **The learning stage (S8) can't move a benchmark number.** We show it as a scripted demo (👎 a comment → the same comment is suppressed on the next push) and say exactly that.

## 4. Linea as a test
Linea is **smaller**, not bigger: 111 files, about 14k lines including lockfiles. The benchmark already has huge repos (Sentry, Grafana, Keycloak, Discourse). Linea has no golden comments and no recorded competitor reviews, so it can't produce a number comparable to Augment or Cursor.

**It is the right repo for two other things:**
1. **A live demo on a real app.** It's a TS monorepo with a client, a server and a shared ws message protocol: real cross-package contracts. The TS graph works on it, and Sunny knows the code well enough to judge every comment.
2. **The seeded cross-file test** (with Recall), pre-registered as above.

## 5. Lanes (19–29 Oct)

| Lane | Tool | Strength used | Jobs |
|---|---|---|---|
| Relay | Claude Code, `~/Projects/tracehound-ops` → `~/Projects/TraceHound` | Long, careful, pre-registered benchmark jobs | 064 (lost) → 065 evidence verifier + gate → 066 dev ablation of v2 → freeze-2 → 067 blind run |
| Codex | ChatGPT Codex, `~/Projects/TraceHound-codex` (**ends 22 Oct**) | Fast, bounded, test-heavy modules | codex-08 gate (19–20) → codex-09 learning (21–22) |
| UI | Codex GPT Sol 6.1, `~/Projects/TraceHound-ui` (**ends 22 Oct**) | Frontend | Sunny's UI changes |
| Claude B (new subscription) | Claude Code, `~/Projects/TraceHound-claude2` (a new worktree) | Second builder | the integration job `int-01` (22–23): wires S1–S8 into the Action as reviewer `th-v2`; takes over UI after 22 Oct |
| Claude cloud | claude.ai/code, $89 | Isolated sessions on GitHub repos; separate sessions stay blind to each other | cc-03 contracts (now); cc-04 seeded test on Linea/Recall (separate session, 20 Oct) |
| Grok | Grok Premium, fresh chat | Live web research with links | grok-07 (now); the final audit (26 Oct) |
| Planning chat | here | Checks every PR, keeps the brain | — |

**Path guard (new):** codex-08 owns `packages/analyzer/src/gate/`; cc-03 owns `packages/analyzer/src/contracts/`; relay 065 owns `packages/analyzer/src/review/verify-evidence*`; codex-09 owns `packages/analyzer/src/learn/` and `packages/action/src/learn.ts`; int-01 owns `packages/action/src/pipeline.ts`, `reviewer.ts`, `action.ts` and `candidate.ts`. Each lane adds **one line** to `bin.ts`; that's the expected merge conflict.

## 6. Ultrareviews (2 left): proposed change
- **#2 → the int-01 branch**, not the 064 branch. int-01 decides what gets posted on other people's PRs and produces the headline number. 064 is scoring scripts whose tables the chat recomputes anyway.
- **#3 → the final pre-submission branch** (27–28 Oct).

## 7. Schedule

| Day | Relay | Codex / UI | Claude B | Cloud | Grok |
|---|---|---|---|---|---|
| Mon 19 | 064 finishes | codex-08 gate · UI | (set up the worktree) | cc-03 contracts | grok-07 |
| Tue 20 | 065 evidence verifier + gate (dev) | codex-08 PR · UI | — | cc-03 PR; cc-04 seeds | — |
| Wed 21 | 065 | codex-09 learning · UI | — | — | — |
| Thu 22 | gate+vote on dev ($0) | codex-09 PR (**last Codex day**) | int-01 starts | — | — |
| Fri 23 | — | — | int-01 PR → Ultrareview #2 | — | — |
| Sat 24 | 066 ablation → **freeze-2** | — | UI | — | — |
| Sun 25 | 067 blind Cal.com + all-50; seeded test run | — | UI | — | — |
| Mon 26 | results + judges' README (R6 fixes) | — | — | — | audit |
| 27–28 | — | — | Devpost, video | Ultrareview #3 | — |
| Thu 29 | **submit** | | | | |

## 8. Risks
- **Codex ends 22 Oct.** codex-09 must land by then, or it moves to Claude B.
- **int-01 is the critical path.** If it slips past 23 Oct, freeze-2 slips and the blind run is squeezed. Fallback: ship whichever stages have merged, and ablate only those.
- **Gating can cut recall the way 056–058 did.** That's why every stage is ablated on dev against pre-set bars, and silence counts as a cost.
- **tsc-based contract checks duplicate the CI** of repos that already typecheck. Their real value is in string-keyed contracts and in unedited files. We say so in the README.
