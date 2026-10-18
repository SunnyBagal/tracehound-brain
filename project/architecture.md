# Architecture and where things live

## Folders on Sunny's Mac

| Folder | What | Who may open it |
|---|---|---|
| `~/Projects/TraceHound` | Main checkout; relay's lane (`build/*`, `docs/*` branches) | Relay; Sunny |
| `~/Projects/TraceHound-codex` | Git worktree of the same repo; Codex's lane (`codex/*` branches) | Codex; Sunny |
| `~/Projects/tracehound-ops` | The relay tool (`src/relay.ts`), briefs in `briefs/`, caps in `ops/lanes.json` | Relay tool; Sunny |
| `~/Projects/tracehound-bench` | Benchmark data, never in any repo: `golden/` (pinned `3f2c8ab`), `recorded/`, `prs/`, `repos/` (bare clones of the dev repos), `reviews/`, `scores/` | Relay builder (dev data only); never Codex, Grok or the chat |
| `~/Projects/tracehound-brain` | This vault | Chat writes; Sunny; others read named notes only |
| `~/Projects/TraceHound-eval`, `~/Projects/tracehound-heldout` | Held-out evaluation material (phase 2) | Nobody, in current work |
| `~/Projects/Recall` | Recall repo | Not opened by TraceHound sessions |

## Code map (`packages/analyzer/src/`)

| Dir | What | Phase |
|---|---|---|
| `extract/` | Deterministic extractors: imports, routes, Redis, Prisma, env, startup calls, BullMQ queues | 1 |
| `group/`, `aggregate/` | Anchor-reachability grouping ([003](../decisions/digest.md#003--group-by-anchor-reachability-not-by-directory-or-community-detection)); edges aggregated from facts | 1 |
| `naming/` | Optional Nano naming pass over facts, with deterministic checks | 1 |
| `changes/`, `impact/` | Declaration-level diff (change sets, 038); impact of a diff (023) | 1 |
| `agent/` | Context packets, lexical ranking, MCP tools (`context`, `query`, `mcp`) | 1–2 |
| `harness/` | Repair harness, Docker sandbox, evaluation runner, reproduce stage | 2 |
| `finder/` | Graph rules that emit bug hypotheses, then claims (051–052) | 3 |
| `bench/` | Benchmark converter, scorer, judge `bench-judge-v1` (054) | 4 |
| `review/` | Reviewers `review-diff-v1` (055) and `review-ctx-v1` (057); verifiers `verify-v1` (056) and `verify-ctx-v1` (058); context builders | 4 |
| `llm/` | Client, budget, cache, ledger, prices | all |

Other packages: `viewer/` (static Next.js, React Flow, ELK, Tailwind) and `packages/action/` (the GitHub Action CLI, which calls `review`). Scripts: `scripts/bench/` (fetch, score, compare, pool).

## Reviewer pipeline today

```
PR (title, description, diff)
  └─ review-diff-v1 (Super): chunks of ≤ 60,000 diff chars → findings on added/context lines
       └─ [058] verify-ctx-v1 (Super): per finding, file ±80 lines + hunks + git grep refs (≤ 24k chars) → keep/drop
  └─ [057] review-ctx-v1 (Super): diff + repo context block (≤ 40k chars per chunk) → findings
  └─ [059, running] pool = diff-v1 ∪ ctx-only findings → verify-ctx-v1 → kept
judge: bench-judge-v1 (Super, temperature 0) maps each comment to a golden comment or none
```

What the Action runs today: **pool-verify-v1** (since #30: diff-v1 + ctx-v1 pooled, then verify-ctx-v1), plus Super summaries (#37), fix suggestions (#38) and an optional data branch (#43). Planned v2: [architecture-v2](architecture-v2.md). (Before #30 it ran `review`, review-diff-v1, only.) It posts one GitHub COMMENT review, capped at 15 findings, skips forks, and rechecks the PR head before posting.

## Key commands

- Reviewer: `pnpm tracehound review --prs <dir> --out <dir> [--model id]`
- Context reviewer: `pnpm tracehound review-ctx --prs d --heads f --repo <git dir> --out …`
- Verifier: `pnpm tracehound verify-ctx --reviews d --prs d --heads f --repo <git dir> --out … --kept …`
- Score: `pnpm tracehound bench score --golden <file> --reviews <dir> --detail <file outside repo>`
- Spend ledger: `pnpm spend`
- Gates: `pnpm test`, `pnpm typecheck`. Run them in a plain shell; the GIT_* env check must print nothing first.

## Frozen artefacts (sha256 prefixes)

| What | sha256 |
|---|---|
| Judge prompt `bench-judge-v1` | `23de4e9e…058460` |
| review-diff-v1 prompt | `f77c2031…dea4849` |
| verify-v1 prompt | `ab2d6105…` |
| review-ctx-v1 prompt | `0c4dcdcf…728089e0` |
| verify-ctx-v1 prompt | `75703fc6…0a383e70` |
| Golden comments commit | `3f2c8ab794b4ed71c099cbf0a7670555e90906dc` |
| Repair eval freeze | tag `eval-freeze-2` (agent-v6 prompt `556861d4…`) |
