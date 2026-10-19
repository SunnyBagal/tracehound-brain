# TraceHound handoff, 19 Oct 2026, 13:45 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer on NVIDIA Nemotron 3 Super via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**. The deadline is 30 Oct 2026, 22:30 IST; the target is to submit on 29 Oct.
- You plan; you don't build. Write every prompt as a **file in the brain** (`prompts/<date>/<job>.md`), push it before Sunny pastes it, and say which tool, folder and session it goes in.
- **Check every PR from GitHub before Sunny merges it:**
  - clone the public repo and fetch `refs/pull/N/head`;
  - read the diff;
  - recompute every table;
  - probe deterministic filters with adversarial inputs;
  - run the tests locally (the sandbox has Node 22; the repo wants 24, which is fine for vitest).
  - CI: WebFetch the checks page. It often shows no conclusion, so say so.
  - Put **HOLD #N** in the first line of your reply when something must not merge.
- **Brain vault:** `SunnyBagal/tracehound-brain`. Attach it with `add_repo` (owner SunnyBagal, repo tracehound-brain, access push).
  - File every agent output Sunny pastes as `prompts/<date>/<job>.out.md`, verbatim, with a one-line header. The filing rule is in the brain's README.
  - Update NOW, prs, results, timeline and lessons after each event, then push.
- Style: short, blunt, honest. Sunny wants direct feedback, not reassurance.

## Read in order
1. `NOW.md` (state, PR checks, prompt queue)
2. `project/architecture-v2.md` + `decisions/P007-v2-architecture.md` (the v2 plan, "proposed"; Sunny hasn't objected)
3. `lessons.md` (top 4 entries are from 19 Oct)
4. `results.md` (now with the 064 table and the **5-run spread**)
5. `research/R8-competitors-learning.md`

## State at handoff (verify on GitHub)
- **Main `5857179`.** Merged on 19 Oct: #43 data branch (`2af8c3a`), #44 dashboard, #46 064 vote (a loss), #47 cc-03 contracts.
- **#45 gate-v1, head `efb61d7`.** codex-08b landed in two commits (`6fe9803`, `efb61d7`). **Checked by the chat at 13:40:**
  - Defect signals now protect real bugs. Probe: the two bug sentences are kept, and "Consider renaming…" and "nit: trailing whitespace" are dropped.
  - Contract and tsc findings are exempt from `style`.
  - Base-side evidence is checked through `git show` with `cleanGitEnv`, `--no-textconv` and path checks. If there's no base reader, the check is **skipped**, not dropped.
  - Null-line findings are kept when their path is in the diff.
  - The usage line now lists `--base`. The gate tests pass locally (122), and the PR merges cleanly with main.
  - Minor, accepted: any defect word keeps a nit ("Consider adding a docstring; error handling here is fine" is kept). That leans toward keeping.
  - **→ Merge once CI is green** (not confirmed at handoff), using a merge commit.
- #41 and #42 are drafts (cc-01 audit, ReviewBench spike); they feed later jobs.
- **Nebius:** ledger $23.17092 against a total cap of $45. `lanes.json` `nebiusCapUsd` is 5 (set by Sunny; relay confirmed it). The console balance was last read on 18 Oct ($39.93), so re-read it.
- **Claude cloud:** about $89 minus cc-03's use (Sunny to check). It expires 5 Nov 13:29 IST. **2 Ultrareviews left:** #2 → int-01 integration branch, #3 → final pre-submission branch.
- **Lanes:**

| Lane | Tool | Where |
|---|---|---|
| Relay | Claude Code | from `~/Projects/tracehound-ops` → `~/Projects/TraceHound` |
| Codex | ChatGPT Codex | `~/Projects/TraceHound-codex`, **ends 22 Oct** |
| UI | Codex Sol | `~/Projects/TraceHound-ui`, ends 22 Oct |
| Claude B | Sunny's second Claude subscription | new worktree `~/Projects/TraceHound-claude2`: int-01 integration, then UI |
| Claude cloud | claude.ai/code | — |
| Grok | fresh chat each job | — |

## Results to know
- **064 vote:** a loss. pool-verify-v1 across 5 dev runs at temperature 0 scores **F 35.6 / 48.0 / 44.0 / 42.0 / 41.7** (mean 42.3, P 40.5) under v1, and mean 51.3 under x1. **Run 1 (the one 059 chose on, and the dev part of the all-50 table) is the lowest.** Comparisons now use 5-run means. The results-wording job reports the all-50 table with the dev mean shown beside it, declared as such.
- **Claims (unchanged):** "on 10 blind PRs, statistically tied with Greptile and Cursor, ahead of CodeRabbit; 4th of 6 across all 50; every loss published". Never "3rd of 6" as a ranking, and never "only TraceHound catches cross-file".
- **cc-03 contracts (#47):** 10 of 10 fixture breaks caught, 0 findings on the 10 controls, 0 findings on 9 real commits (that says nothing about recall), excalidraw at 53–65 s against a 120 s budget.

## Decisions on 19 Oct
- The v2 architecture (P007): pre-filter → contracts → generators → evidence verifier → deterministic gate → deliver → learn. **Voting was dropped** (064). **No standalone Cal.com run**; there is one blind Cal.com run of full v2 after **freeze-2 (24 Oct)**, stated as the second look at that set.
- **Sunny keeps CodeRabbit** in the seeded test and keeps its public numbers. The chat flagged §4.2(iv) of CodeRabbit's terms (no publishing of benchmark results) once. Don't re-argue it.
- Linea is not a benchmark (no golden comments). Linea and Recall get public mirrors for the **seeded cross-file test** (cc-04) and serve as demo repos.

## In flight / next (prompt files in `prompts/2026-10-19/`)

| # | Job | Lane | Status |
|---|---|---|---|
| 1 | Merge #45 when CI is green | Sunny | ready |
| 2 | `065-evidence-gate.md` | Relay, cap $5 | Run 1 stopped at step 0 ($0; #45 wasn't merged). The brief is patched (it checks the usage line, not `--help`). After #45 merges, Sunny re-copies it with `cp ~/Projects/tracehound-brain/prompts/2026-10-19/065-evidence-gate.md ~/Projects/tracehound-ops/briefs/` and restarts `caffeinate -i bun src/relay.ts start tracehound briefs/065-evidence-gate.md`. When the output arrives, check: freeze before the first call; the 5-run means; the **drop log per gate rule with TPs lost**; the adoption rule as written; no edits under tracehound-ops |
| 3 | `cc-04-seeded-test.md` | Claude cloud, new session on `SunnyBagal/tracehound-seeds` | Sunny is creating the 3 repos (commands at the top of the prompt). **Updated at 13:35 to include CodeRabbit** (with a manual `@coderabbitai review` trigger and ≤ 8 PRs/hour). If Sunny started the session before that, he must tell it about the change. Check: 30 seeds, verify.ts all passing, no hint words, the sha256 of key.json and SCORING.md (record it in NOW publicly) |
| 4 | codex-09 learning (S8) | Codex, **must land by 22 Oct** | **Not written yet.** Write it next. Inputs from R8: read `AGENTS.md`, `CLAUDE.md`, `.cursor/BUGBOT.md`, `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md` (applyTo) and `.tracehound/rules.md`; feedback from 👍/👎, threads resolved without a commit touching the line, and replies → fingerprints on `tracehound-data/feedback/`; lexical matching (embeddings optional, behind a flag); implements the gate's `Suppression` interface; owns `packages/analyzer/src/learn/` + `packages/action/src/learn.ts`; adversarial fixtures in the brief |
| 5 | int-01 integration | Claude B, 22–23 Oct | Not written. Wires S1–S8 into the Action as reviewer `th-v2`; owns `packages/action/src/{pipeline,reviewer,action}.ts` + `packages/analyzer/src/candidate.ts`; the review body format is "What breaks / When / Evidence / Fix"; then **Ultrareview #2** |
| 6 | 066 dev ablation → freeze-2 (24 Oct) → 067 blind Cal.com + all-50 + seeded test (25 Oct) | Relay | after int-01 |
| 7 | Results wording fix + judges' README (R6's 5 fixes, the all-50 table with the dev 5-run mean, the source of CodeRabbit's numbers) | Relay or Claude B | 26 Oct |
| 8 | X post | Sunny | Planned for Mon 19 Oct. Sunny will bring Grok's grok-04 draft "later". Check it against results.md and the agreed claims |
| 9 | UI changes | UI lane | Sunny will describe them later |
| 10 | Grok final audit (26 Oct), Devpost, video under 3 min, **submit 29 Oct** | — | — |

## Known limits to keep stating
- Temperature 0, while NVIDIA recommends 1.0 / top_p 0.95 ("across all tasks"). We won't change it before freeze-2; it's listed as a limit.
- Bugbot is usage-based at about $1–1.5 per run, so 30 seeded PRs ≈ $30–45 unless the trial covers them. That's Sunny's call.
- Tool terms other than CodeRabbit's are unread. Sunny reads Greptile's and Bugbot's before publishing.

## Side project
**TariffLens** starts 30 Oct (Sunny's AI portfolio project).
