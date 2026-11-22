# TraceHound audit and roadmap

Written 16 Nov 2026 from `main` @ `1986f51` and `origin/claude/upbeat-lamport-b280zm` @ `ccf6c01`.
Read-only: nothing in the repository was edited, committed or pushed, and no model API was called.

Every statement carries one of four tags:

- **VERIFIED**: I ran the check or read the code today; the method is named.
- **DOCUMENTED**: a repo document or a web page says so; I did not check it independently.
- **INFERRED**: my reasoning from verified or documented facts.
- **UNVERIFIED**: could not be checked.

Web pages were read through a fetch tool that returns a summary of the page, so a DOCUMENTED web
fact is "the page as summarised to me", not a raw quote, unless the text is quoted.

---

## 1. Where this report disagrees with the handover

The repository wins in each case.

| Handover says | Repository says | Tag |
|---|---|---|
| Terry's scores for five side-track folders are pending | All five are scored: 17 of 31 each. A/B 1: arm A mean 17.0, arm B mean 17.0, union 19 each (decision 076o; `tracehound-testset/run/out/bunny.076o.md`) | VERIFIED (read both) |
| Tree equality of case heads is unverified | Amy checked: 40 of 40 dev case-head trees equal the commits the side track reviewed (`.agents/LOG.md`, 15 Nov) | DOCUMENTED |
| Analyzer unit suite 1,013 passed | Today: 1,075 passed, 6 skipped, 1 failed in the full workspace run; the failing test passes alone | VERIFIED (ran `pnpm test`) |
| The JSON fixes exist only on the side branch | The lenient reply parser has been the default on `main` since decision 073 (`review.ts:177`). It is not in the published Action: the workflow pins `86fd5d7` (27 Oct), which has no `parser` option. The side branch does not fix the pipeline's JSON handling; it replaces the pipeline | VERIFIED (`git show 86fd5d7:…/review.ts`) |
| Bugbot's "8-pass voting" was dropped as unsourced | Cursor's own post describes it: eight parallel passes with randomised diff order, majority voting, then a validator. It was the launch design and was later replaced by an agentic one | DOCUMENTED (cursor.com/blog/building-bugbot, 15 Jan 2026) |
| Check STATE for the held-out window | `.agents/STATE.md` is stale: its header says "after 076k" while `main` is at 076o. No held-out result exists in the repo. Scheduled T0 is between 17 and 22 Nov. I read no pull, issue or notification on the four forks | VERIFIED (read STATE, decisions, LOG) |
| Sunny says model selection is open | `AGENTS.md:102` still says product model calls go to Nemotron, and `AGENTS.md:142` makes any other provider a P1 review finding. The only exception is report-only Claude rows. **This needs a ruling from Sunny, recorded by Holt** | VERIFIED (read) |

---

## 2. Architecture

### 2.1 What is in the repo

| Part | Path | Size | State |
|---|---|---|---|
| Analyzer (component graph) | `packages/analyzer/src/{extract,group,aggregate,load,naming}` | part of 18,810 lines | Built; publishes snapshots |
| Viewer | `viewer/` | 225 tests | Live on Vercel |
| Repair harness, finder, reproduce | `packages/analyzer/src/{harness,finder}` | | Built; evaluation done (decision 050) |
| PR reviewer | `packages/analyzer/src/review`, `packages/action`, `.github/actions/tracehound-review` | | Shipped as an Action at `review-freeze-2.1` |
| Contracts check | `packages/analyzer/src/contracts` | | In the Action, on by default |
| Benchmark and test-set tooling | `packages/analyzer/src/bench`, `scripts/{bench,testset,reviewbench}` | | In use for the held-out run |

The reviewer does not use the component graph. Nothing under `packages/analyzer/src/review/` imports
graph code: its imports are `../llm/*`, `../bench/types.ts`, `../naming/llm.ts`, `../git-env.ts`, a
type from `../agent/decider.ts`, and `scripts/bench/agree-rule.ts`. **VERIFIED** (grep of every import
in the directory). The contracts check is the one place analyzer extractors reach a review:
`contracts/keyed.ts` calls the BullMQ, HTTP, Redis and env extractors. **VERIFIED** (read).

### 2.2 The review pipeline, end to end

All line references are on `main` @ `1986f51`. **VERIFIED** by reading each file unless marked.

1. **Trigger** (`.github/workflows/tracehound-review.yml`). A `tracehound-review` label or a manual
   dispatch with a PR number. Same-repository, non-draft PRs only. One run per PR at a time. Checkout
   at the head sha with `persist-credentials: false`. The action is pinned to `86fd5d7`.
2. **Composite action** (`action.yml`). Node 24, `npm install -g pnpm@11.20.0`,
   `pnpm install --frozen-lockfile --ignore-scripts`, then `node packages/action/src/cli.ts`.
3. **Entry** (`packages/action/src/cli.ts`). Validates inputs. Returns 1 if `NEBIUS_API_KEY` is
   missing (`:42`). Writes a usage table to the job summary whatever happens.
4. **`runAction`** (`action.ts:360`). Refuses `calcom/cal.com`, skips fork PRs, checks the head commit
   is present, fetches the diff from the GitHub API, confirms the head has not moved, and confirms it
   again before posting. Posts one `COMMENT` review; on HTTP 422 it retries as body-only.
5. **`prepareReview`** (`action.ts:274`). Starts the contracts check in parallel, runs the pipeline,
   drops model findings within 3 lines of a contract finding, applies `learn` (off by default), caps at
   `max-comments`, asks for fixes, builds the payload and a machine-readable footer.
6. **`runPipeline`** (`pipeline.ts:127`). Two analyzer subprocesses in parallel (`review`,
   `review-ctx`), a pool step, then a `verify-ctx` subprocess. Every count is cross-checked; any
   mismatch posts nothing.
7. **Chunking** (`review/diff.ts`). The whole diff is one chunk up to 60,000 characters; otherwise
   files are packed and an oversized file is cut at hunk boundaries. There is no file filter.
8. **Diff reviewer** (`review/review.ts`). One call per chunk, temperature 0, `max_tokens` 32,000,
   reasoning on, one retry on an invalid reply. Findings are checked against the diff's new-side lines.
9. **Context reviewer** (`review/review-ctx.ts`, `context.ts`, `names.ts`). The same loop plus a
   context block of at most 40,000 characters: the changed files, then `git grep -w` hits for names
   that regular expressions pull out of the diff. When the block is too big, references are cut first
   (`context.ts:251`).
10. **Verifier** (`review/verify-ctx.ts`). One call per pooled finding with up to 24,000 characters of
    context; keep or drop. A failed verification keeps the finding.
11. **Model client** (`llm/client.ts`). Cache, then a worst-case budget reservation, then the request,
    then the ledger. No retry or backoff on the Nebius path. A network failure is ledgered at the
    upper bound (`:294`).
12. **Budget and ledger** (`llm/budget.ts`, `ledger.ts`). Run cap $1 and total cap $45 by default,
    per process. The ledger is an append-only JSONL file.
13. **Contracts** (`action/src/contracts.ts`, `analyzer/src/contracts`). A subprocess with GitHub and
    Nebius credentials removed from its environment, a 90-second budget, and a typed stage capped at
    5,000 root files (`typed.ts:12`).
14. **Fixes** (`action/src/fix.ts`). One call per posted finding, on a hard-coded model
    (`FIX_MODEL`, `:15`). Each fix is validated against the diff and must be committable.

### 2.3 The side branch

Twelve commits on top of `9bf04b2`, 292 files, 4,684 added lines; 282 of the files are under `sidetrack/runs/` (280 reviews in 7 folders).
**VERIFIED** (`git diff --stat`).

| File | What it is | Read |
|---|---|---|
| `packages/analyzer/src/affected/affected.ts` | For each declaration with added lines, compiler-resolved references at head; loads only files that mention the name | In full |
| `.claude/skills/tracehound-review/SKILL.md` | Four steps: collect facts, look inside and outside the diff, validate by quoting both lines, print JSON; at most 6 comments | In full |
| `scripts/track-c/review-case.ts` | Runs `claude -p` with Read, Grep and Glob only; writes facts, a review and a run record | In full |
| `sidetrack/ab1-plan.md` | Pre-registered short-packet A/B | In full |
| `affected/typed-narrow.ts`, `scripts/track-c/score.ts`, the two test files | | Not read |

It is a different kind of reviewer from the product: an agent that opens files, against a pipeline
that makes fixed calls. That difference matters for reading its result (section 3, S2).

---

## 3. Findings

Severity is about practical impact on the reviewer as a product or on the validity of its numbers.

### Product reviewer

| ID | Sev | Finding | Where | Impact | How checked |
|---|---|---|---|---|---|
| R1 | High | Cross-file context is weak and is the first thing cut. Names come from regular expressions, references from `git grep -w`, and the budget drops references before anything else | `review/names.ts`; `context.ts:204-231`, `:251-255` | The reviewer rarely sees callers. References survived in 12 of 48 dev chunks (decision 057). On seeded PRs it caught 4 of 16 cross-file defects, 3 of them from the contracts check, and 0 of 4 semantic ones (`README.md:164`) | VERIFIED (code read); counts DOCUMENTED |
| R2 | High | No retry on a transient request failure. A network error, 5xx or 429 fails the chunk at once, and the call is ledgered at the upper bound | `llm/client.ts:284-299`, `:350-356`; `review/review.ts:225-230` | Partial reviews from brief outages, and phantom spend. On 14 Nov, 17 failed requests added $0.57 to the ledger that Nebius never charged (`docs/budget.md`) | VERIFIED (code read); incident DOCUMENTED |
| R3 | High | The published Action is three weeks behind `main`. The pin `86fd5d7` has the strict parser; the lenient parser that won decision 073 is not shipped | `.github/workflows/tracehound-review.yml`; `review.ts:177` | Users get the invalid-JSON rate the README reports (about 7 of 40 PRs partial). STATE lists the pin bump as open task g | VERIFIED (`git show` at the pin) |
| R4 | Med | The retry still treats a reply cut off at the token limit like malformed JSON. The default appends the cut-off reply and asks again under the same cap | `review.ts:232-241` | A truncated chunk usually fails twice. The alternative ("fresh") lost in 073, so this is known and open | VERIFIED (code read) |
| R5 | Med | The contracts typed stage does not run on large monorepos: a 5,000-root-file cap and a 90-second budget | `contracts/typed.ts:12`, `:195`; `action/src/contracts.ts:10` | It finished on 3 of 40 dev cases (decision 076n). The stage that produced most seeded cross-file catches is absent where it is needed | VERIFIED (code read); count DOCUMENTED |
| R6 | Med | The product is tied to one provider. No key, no run; the fix stage ignores the `model` input | `action/src/cli.ts:42`; `action.yml`; `fix.ts:15` | A user cannot bring an Anthropic or OpenAI key. The review header names the review models while fixes come from another | VERIFIED (code read) |
| R7 | Med | Every trigger posts a complete new review. Nothing reads earlier TraceHound reviews on the PR | `action.ts:360-418` | Re-labelling after a push repeats comments and repeats the cost | VERIFIED (code read) |
| R8 | Med | No file filter before chunking: lockfiles, generated and vendored files are reviewed like source. Very large diffs fail as a generic "action failed" | `review/diff.ts:113`; `action.ts:381` | Wasted spend and noise on big PRs. GitHub's exact diff-size limit is UNVERIFIED | VERIFIED (code read); limit INFERRED |
| R9 | Low | Budget caps are per process. One review runs three analyzer processes and a fix client, each with its own $1 run cap, and concurrent processes do not see each other's reservations | `review/cli.ts:73`; `fix.ts:29`; `llm/budget.ts` | The per-review ceiling is about $4, not $1. The total cap can be overshot in parallel. The team's "one paid process at a time" rule covers evaluation runs | VERIFIED (code read) |
| R10 | Low | One malformed ledger line stops every CLI: `entries()` parses each line with no guard | `llm/ledger.ts:36-42` | A crash mid-append blocks all later runs until the file is repaired by hand | VERIFIED (code read) |
| R11 | Low | `review-ctx.ts` repeats `review.ts`'s request, retry and validation loop | `review-ctx.ts:65-134` | Every fix to the loop has to be made twice | VERIFIED (code read) |
| R12 | Low | `git grep` and `git show` run synchronously with no timeout | `context.ts:47` | A slow grep stalls the step until the job's 20-minute limit | VERIFIED (code read) |
| R13 | Low | Model text is posted unescaped; contract text is escaped. Analyzer subprocesses inherit `GITHUB_TOKEN`, which they do not need | `action.ts:184`; `contracts.ts:163`; `pipeline.ts:146` | Low today, because only same-repository authors reach it. High if fork PRs are ever supported | VERIFIED (code read) |
| R14 | Low | `actions/checkout@v4` and `setup-node@v4` are pinned by tag, and pnpm is installed without an integrity check | `action.yml`; workflow | Ordinary supply-chain exposure | VERIFIED (read) |

### Repository and process

| ID | Sev | Finding | Where | How checked |
|---|---|---|---|---|
| P1 | Med | `pnpm test` fails intermittently on `main`. Today: `test-set-run.test.ts` › "refused, with nothing written…" timed out at 5,000 ms in the full run and passed alone (58 of 58). Terry measured 4 failures in 20 full runs and has a test-only fix in PR #78, not merged | `packages/analyzer/test/test-set-run.test.ts:461`; `run/out/bunny.overnight-2026-11-15.md` | VERIFIED (ran both); fix DOCUMENTED |
| P2 | Med | The instruction files are out of date. STATE's header stops at 076k. CLAUDE.md's "Current state" is dated 6 Oct and does not describe the Action, contracts, the 3.5 default or the TypeScript track | `.agents/STATE.md:2`; `CLAUDE.md` | VERIFIED (read) |
| P3 | Med | Prices for the Nebius models are unverified and the ledger runs about 15% low on real calls. Decision 076k item 10 requires this fixed before the tag | `config/prices.json`; `.agents/LOG.md` 14 Nov | DOCUMENTED |
| P4 | Low | The side branch is on a public repository and holds 280 review outputs with dev case ids and PR titles | `sidetrack/runs/2026-11-15/` | VERIFIED (`gh repo view`: PUBLIC; one file's keys) |
| P5 | Low | Local Node is v26.13.1; the viewer declares 24.x. 42 git worktrees exist. `run/` is untracked in the repo root | | VERIFIED (ran); worktrees DOCUMENTED |

Anthropic prices in `config/prices.json` match the live pricing page, including Haiku 5.5's higher
tier above 100,000 prompt tokens. **VERIFIED** (compared with platform.claude.com today).

### Side branch

| ID | Sev | Finding | Where | How checked |
|---|---|---|---|---|
| S1 | Med | A change that only deletes lines is invisible. The lookup marks a declaration as changed only when it has added lines, and a deletion-only hunk has none. Removing a guard clause is a common way to break callers | `affected.ts:214`; `changes/git.ts` `parseZeroContextDiff` | VERIFIED (ran the parser on a deletion-only hunk: `added: []`) |
| S2 | Med | The Sonnet result cannot be attributed. Against A0 the arm changes the model, adds file-reading tools, changes the prompt and adds the lookup, all at once. Haiku with the same skill scored 12 (no gain). Sonnet inside the unchanged pipeline scored 2 of 8, against 4 of 8 for the skill on the same cases | decisions 076j, 076n, 076o | DOCUMENTED |
| S3 | Med | Training-data exposure is unchecked and plausible. Claude 5.5 models have a training cutoff of June 2026; the forks sit at the last upstream commit before 1 July 2026, so fixes for the dev bugs may be in the training data. Six Sonnet draws all landing on exactly 17 fits either a stable ability or recall | models overview page; `.agents/STATE.md` "Track B" | Cutoff DOCUMENTED; exposure INFERRED |
| S4 | Low | More comments, flat precision. Sonnet draws posted 54 to 64 comments on 40 cases against A0's 43, and 2 to 4 comments on clean cases against 1. Catches per comment: about 17 of 57 against 12 of 43 | `bunny.076o.md` | DOCUMENTED; ratio is my arithmetic |
| S5 | Low | The cost and token numbers have no surviving record. The runner writes them to an output directory that was not kept | `review-case.ts` step 4 | VERIFIED (code read); loss DOCUMENTED |
| S6 | Info | The runner uses the signed-in Claude Code account. That is fine for Sunny's own experiments. It cannot be how a product authenticates: Anthropic's page says developers building products "should use API key authentication" and may not route requests through plan credentials for their users | code.claude.com/docs/en/legal-and-compliance | VERIFIED (read the page today) |

---

## 4. Models

Primary pages read today. Availability on Sunny's accounts is **UNVERIFIED** (no API call was allowed).

### Anthropic (platform.claude.com, pricing and models overview) — DOCUMENTED

| Model id | Input / output per MTok | Cache read | Batch | Notes |
|---|---|---|---|---|
| `claude-fable-5-2` | $8 / $40 | $0.20 | $4 / $20 | Thinking always on |
| `claude-opus-5-5` | $4 / $20 | $0.20 | $2 / $10 | Default effort medium |
| `claude-sonnet-5-5` | $2 / $10 | $0.10 | $1 / $5 | |
| `claude-haiku-5-5` | $0.10 / $0.50 up to 100,000 prompt tokens; $0.50 / $2.50 above | $0.01 / $0.05 | $0.05 / $0.25 | The higher price applies to the whole request |

All four: 1M-token context, 128K output, training cutoff June 2026. Batch is 50% off and combines with
caching. A non-default temperature is rejected, so the project's "temperature 0, one run" habit does
not carry over: every Claude arm needs at least two draws.

### OpenAI (developers.openai.com, pricing and models) — DOCUMENTED

| Model id | Input / cached / output per MTok | Notes |
|---|---|---|
| `gpt-6-astra` | $10 / $1 / $50 | Flagship for reasoning and coding |
| `gpt-6.1-sol` | $2 / $0.10 / $10 | "Near-Astra" at lower cost |
| `gpt-6.1-terra` | $1 / $0.05 / $5 | Fast; computer use |
| `gpt-6-luna` | $0.10 / $0.01 / $0.50 | High-volume |
| `gpt-5.3-codex` | $1.75 / $0.175 / $14 | Codex line |

The four GPT-6 models: 1.05M context, 128K output, knowledge cutoffs 30 Apr to 1 Jun 2026. Batch and
flex are half price. Inputs above 272K tokens cost more.

### Fit for review

- **What the repo has measured** (DOCUMENTED, decisions 076j to 076o; dev, one judge):
  Haiku 5.5 inside the unchanged pipeline caught 14 and 11 of 31 against A0's 12, at a median $0.007
  per case against $0.045, with 0 failed chunks against A0's 2 of 84. Sonnet 5.5 inside the pipeline:
  2 of 8 against A0's 3 of 8, at $0.17 per case. Sonnet as an agent with the skill: 17 of 31.
- **Reading** (INFERRED): a model swap inside the fixed pipeline buys reliability and cost, not
  catches. The only gain came when the model could open files itself.
- **A trap**: the side track's Sonnet runs read 75,000 to 92,000 input tokens each (handover figure,
  UNVERIFIED). An agentic Haiku run of that size sits just under the 100,000-token line where Haiku's
  price rises five times. Any Haiku agent needs a hard input cap below it.
- **Structured outputs** exist on both providers. They remove the invalid-JSON failure class that
  costs the Nemotron pipeline about 7 partial reviews in 40. (Anthropic: DOCUMENTED in the bundled API
  reference; OpenAI: UNVERIFIED today.)
- **What vendors say**: Augment writes that "the GPT model series has consistently performed the best
  so far" for its reviewer (DOCUMENTED, its blog, 10 Mar 2026). Cursor's post names no model.

---

## 5. Greptile, CodeRabbit, Cursor, Augment

| | Kind | Documented mechanism | Documented price | Self-reported quality |
|---|---|---|---|---|
| **CodeRabbit** | PR-review product; also IDE, CLI and a Slack agent | Linters and SAST tools on every paid plan; MCP connections; issue-tracker and CI context | $24, $48, $72 per developer per month billed annually; 5, 8, 10 PR reviews per hour per developer; Free plan 1 per hour, summary only | None read today |
| **Greptile** | PR-review product | "Builds a graph of your entire repository"; learns from reactions over 2 to 3 weeks; per-directory rules; an end-to-end testing agent (TREX) | $1 per credit; a review is 1, 3 or 10 credits by tier; TREX run 3 credits; self-hosting on Enterprise | None read today |
| **Cursor Bugbot** | A review feature inside a coding assistant | Launch: 8 parallel passes, majority voting, validator. Since autumn 2025: one agent with tools and dynamic context, which gave "the largest gains". The agent was "too cautious", so prompts were made more aggressive | Usage-based; no dollar figure in the docs | Resolution rate 52% to over 70%; 0.4 to 0.7 bugs flagged per run; over 2 million PRs a month |
| **Augment** | A coding-assistant company with a review product | Semantic "Context Engine" retrieval; tools for retrieval, file browsing, symbol search; large inputs injected deterministically; per-directory guidelines | Not read | F1 53.8% on Code Review Bench; 1.03 bugs fixed per PR against 0.54 for humans; 45% of comments addressed; 3.6 comments per PR |
| Anthropic Code Review | Part of Claude Code | A team of agents, then verification | $15 to $25 per review; about 20 minutes | Under 1% of findings marked incorrect by engineers |

All rows **DOCUMENTED** from the vendors' own pages read today (docs.coderabbit.ai/management/plans,
greptile.com/pricing and docs, cursor.com/blog/building-bugbot and docs, augmentcode.com blog,
claude.com/blog/code-review). Every quality number is the vendor's own.

**UNVERIFIED**: which models CodeRabbit and Greptile use; whether CodeRabbit keeps a code graph or
runs code in a sandbox; Greptile's internals beyond one sentence; funding, market share and star
counts from Gemini's material; anything about Copilot or Graphite; the Martian leaderboard (its page
returned no numbers).

What the documented facts support (INFERRED):

1. Both vendors that describe their design in detail (Cursor, Augment) run an agent with repository
   tools. Neither describes a fixed chunk-and-call pipeline as its current design.
2. Vendors steer by a production metric: the share of comments that authors act on. TraceHound has no
   users, so it has no such number.
3. Self-reported F1 on the public benchmark sits between about 45% and 60%. TraceHound's blind
   Cal.com F of 57% to 58% on 23 golden comments is inside that band and cannot be ranked within it.
4. The benchmark TraceHound used includes Cal.com, and Martian's own README says static sets risk
   training leakage. The same caution applies to TraceHound's dev set (S3).

---

## 6. Viability

### Technical

- **Sound**: the evaluation method (pre-registration, frozen and hashed prompts, two judges, paired
  tests, counts beside every rate). I did not see a vendor publish anything as careful. The Action's
  handling of head changes, credentials and forks is also careful. VERIFIED by reading.
- **Saturated**: the fixed Nemotron pipeline. Nine decisions (056, 058, 064, 065, 070, 071, 073 to 075) and levers v2 lost under pre-registered gates. On the blind
  set the full pipeline and the plain diff reviewer are tied (F 58.3% against 53.8%, n = 23). On the
  TypeScript dev set it catches 12 of 31 (Wilson 24% to 56%). DOCUMENTED.
- **One open signal**: Sonnet as an agent, 17 of 31, +5 over A0, p about 0.18, on a selection set,
  confounded (S2) and possibly contaminated (S3). It is a reason to run a proper test and nothing more.
- **The graph**: no measured benefit anywhere. The repair agent made 0 graph tool calls in 80 runs.
  The reviewer does not read the graph. Queue pairing was 2.1% on unseen repos. DOCUMENTED.

### Commercial

- A review costs TraceHound about $0.05 (pipeline) to $0.12 (Sonnet agent). CodeRabbit charges $24 or
  more per developer per month and Greptile $1 per review. **Cost is not the constraint.** INFERRED.
- The constraints are quality that can be shown, and distribution: an Action that needs a Nebius key,
  works on same-repository PRs only, and has no users.
- The strongest competitors are the model vendors and the editors, which bundle a reviewer.

### Assessment

Leading review quality across the board is not a credible goal on the evidence here. A credible,
narrower route exists, and it should be abandoned if the tests below go against it:

1. **Claim one thing**: breakage in files the PR did not touch, in TypeScript monorepos, with the
   evidence shown. This is where the contracts check and the lookup are distinctive and cost nothing
   to run.
2. **Ship it where a user's own credentials already work**: a skill and a documented workflow for
   Claude Code and its official GitHub Action, plus a bring-your-own-key Action.
3. **Make the published, pre-registered evaluation the asset.**

Stop conditions, to be agreed before the numbers arrive:

- The held-out result puts A0 six or more catches behind CodeRabbit (the project's own RETHINK line),
  **and** the Sonnet agent fails to keep a gain of 4 or more on a fresh set (E1 in section 8).
  Then: stop company work; keep the repository as research.
- The no-lookup arm (A1 below) matches the lookup arm. Then: the lookup is not a product feature;
  keep it as an optional tool and say so.

---

## 7. Graph-based context: an objective test

### What is known

No experiment has isolated graph or symbol context from the model and the agent loop. Outside
evidence points both ways: Greptile and Augment sell an index or graph as their core; SWE-PRBench
(arXiv 2603.26130, one author, 350 PRs, 8 models) found that all models got worse as more context was
added to a single call; Cursor reports its largest gains from letting an agent fetch context itself.
All DOCUMENTED.

### Ablation A1 (first), then A2

Same model, same skill text apart from the facts step, same tools, same 40 dev cases, same cap of
6 comments, two draws per arm, judged by bench-judge-v1 with x1 alongside.

| Arm | Context | Status |
|---|---|---|
| A1-none | Agent with Read, Grep, Glob; **no** lookup packet | **Missing. Run first.** |
| A1-lookup | The same plus the affected set (short packet) | Run: 17, 17 |
| A2-graph | The same as A1-none plus a `tracehound context` packet from a snapshot at base | Not built |
| A2-hybrid | Lookup and graph packet | Not built |
| A2-tool | No packet; the lookup offered as a tool the agent may call | Not built |
| A0, Haiku and Sonnet pipeline | Fixed pipeline rows, already scored | Reference only |

Counted for every arm, per case: construction time cold and warm, peak memory, declarations skipped,
whether the build failed, input and output tokens from the provider's usage record, wall time, cost.
A graph arm also reports snapshot build time and how stale the snapshot is against the PR base.

Decision rule, written down before the run:

- A difference under 4 catches of 31 between arm means is "no detectable change".
- Lookup stays in the reviewer only if A1-lookup beats A1-none by 4 or more.
- A graph arm is adopted only if it beats A1-none by 4 or more and costs at most 1.5 times as much.
- Otherwise the cheaper arm is kept and the numbers are published.

My prediction, stated so it can be wrong: A1-none lands within 3 of A1-lookup, and no graph arm
clears the bar.

**Limit**: with 31 bug cases, only a gap of about 8 or more is statistically clear (+5 gave p 0.18).
A1 can show that a large effect is absent. It cannot rank close arms. Only 3 of the 31 dev bugs are
multi-file, so dev says almost nothing about cross-file value; that needs set E2 below.

---

## 8. Evaluation design

| Set | Purpose | Size | Source | Status |
|---|---|---|---|---|
| Dev (TS) | Selection | 31 bug, 9 clean | Exists | Heavily used: at least 10 looks |
| Held-out (TS) | The registered test | 20 bug, 9 clean | Exists | One look, A0 and Haiku rows only |
| **E1 fresh** | Confirm any new arm on unexposed code | 40 bug, 15 clean | PRs merged after 1 July 2026 in repos outside the four used so far, built with the existing lineage tooling | To build |
| **E2 seeded cross-file** | Cross-file recall | 30 seeds, 10 clean controls | New seeds by a written rule; never the held-out seed set | To build |
| **E3 clean** | False-positive rate | 30 clean PRs | Merged PRs with no later fix within 60 days | To build |

Measures, each with its count: catches of N with a Wilson interval; comments per PR; share of clean
PRs with any comment; precision twice (golden match, and a blind hand check of at most 40 unmatched
comments); severity agreement as a confusion matrix over caught cases, once labels carry a
three-level severity; wall time p50 and p90; cost per PR from provider usage, with the ledger checked
against the console.

Rules carried over from the project: a plan committed before any run; two draws per arm at least;
paired counts with an exact sign test; a stated noise floor (under 4 of 31; for other sizes, twice the
measured difference between two draws of the same arm); bench-judge-v1 as scorer of record with x1
beside it; a blind check of judge disagreements.

Added for the new models: for each case, the fix commit's date against each model's training cutoff,
with exposed and unexposed cases reported separately.

With 9 clean cases, zero comments still leaves an upper bound near 30% on the false-positive rate.
That is why E3 exists.

---

## 9. Routing, context, caching, indexing, verification, stopping

Each row gives the measurement that decides it. None is worth building before quality is established
(section 6: cost is not the constraint).

| Lever | Evidence so far | Proposal | Accept if |
|---|---|---|---|
| Model routing | Haiku pipeline matches A0 at one sixth of the cost; Sonnet agent +5 | Deterministic skip of docs-only and generated-only PRs; Haiku triage; Sonnet agent on the rest | A replay on dev loses at most 1 catch of 31 and cuts cost by 30% or more |
| Context size | The short packet halved the packet (68 to 32 lines) and changed nothing: 17.0 against 17.0; cost did not move because the agent reads 75,000 to 92,000 tokens anyway | Cap what the agent reads (turns, tokens), not the packet | Median input tokens fall 30% with a loss under 4 of 31 |
| Prompt caching | Cache reads cost 5% of input on Sonnet 5.5. Whether the side-track cost already includes caching is UNVERIFIED | With an API loop: diff and packet in the cached prefix; check `cache_read_input_tokens` | Cache-read share above 60% of input tokens |
| Batch | 50% off on both providers | Use for judging and single-call arms in evaluations; it does not suit tool loops | Evaluation cost halves; results unchanged |
| Indexing | The lookup is built per PR in a median 3.5 s after its rewrite | No persistent index | Revisit only if p90 build time passes 60 s |
| Incremental review | None; the footer already records each posted finding | Review only new commits on re-trigger; carry earlier findings forward | No repeated comment on a re-run fixture |
| Verification pass | A model verifier lost three times: it cut 10 of 32 true positives (058) and 45 to 35 (levers v2) | No second model call. A deterministic quote check only: the lines a comment quotes must exist at the stated `file:line` | Offline on the 7 scored folders: drops under 2 true positives per folder and at least 20% of unmatched comments |
| Stopping | The skill caps comments at 6 | Add a turn cap, an input-token cap (under 95,000 for Haiku) and a wall-time cap; a stopped review says it is partial | No run exceeds the caps |

The quote check touches lost lever 065 (a model that had to quote the failing code). The reason it
could differ: it is deterministic, costs nothing, and checks quotes an agent already wrote instead of
asking a second model to produce them. It is measured offline first for that reason.

---

## 10. Testing TraceHound itself

| Kind | Gap | Test to add | Acceptance |
|---|---|---|---|
| Integration | Action tests inject fakes at each boundary. I found no test that runs `cli.ts` through real analyzer subprocesses (INFERRED from a grep of the test files) | A local fake model server and fake GitHub server; run `node packages/action/src/cli.ts` on a fixture repo and PR | Runs offline in under 30 s; posted payload equals a snapshot; exits 0 |
| End to end | No scheduled run of the real Action | A manual-dispatch dry run on one fixture PR in a scratch repository | Job summary shows a review and a cost under $0.10; run after every pin bump |
| Regression | The identity snapshot covers the happy path | Diff fixtures: deletion-only hunk, rename, binary, CRLF, a hunk over 60,000 characters, quoted paths, empty diff | Each has an expected chunk list and valid-line set |
| Failure recovery | R2, R10 | Client: 429 with `Retry-After`, 5xx, connection reset, timeout. Ledger: a truncated last line | A single transient error does not fail the chunk; the ledger holds one charged row; a bad line is skipped with a warning |
| Security | R13 | Diff fixtures that try to forge the footer, close a code fence, add `@mentions`, or give instructions; an environment with fake tokens | Posted text is neutralised; only the true last-line footer parses; no log line or payload contains a token |
| Flakiness | P1 | Merge PR #78; a nightly job that runs `pnpm test` five times | 0 failures in 5 for a week |
| Performance | The lookup once used 11.4 GB | A synthetic repo of 20,000 files; run the lookup under a 4 GB heap | Finishes under 60 s without an out-of-memory error |

---

## 11. Roadmap

Ownership follows `AGENTS.md`: code in `packages/` and `scripts/` is built by Terry on a branch,
reviewed by Amy, merged by Holt. This list is input for Holt, not a set of edits.

Order: phase 0 now; phase 1 after held-out scoring (decision 076m keeps the side track unmerged and
unpaid until then); phase 2 needs Sunny's ruling on the model rule; phase 3 is gated on phase 1.

### Phase 0: now, $0, nothing frozen is touched

**T0.1 Merge the flaky-test fix.** Depends on: Amy's GO on PR #78. Touches:
`packages/analyzer/test/test-set-run.test.ts` only. Check that no file pinned by the P1 scheduler is
in the diff (list in LOG, 14 Nov). Test: `for i in 1 2 3 4 5; do pnpm test || break; done`.
Accept: five green runs. Rollback: revert the merge commit.

**T0.2 Bring STATE.md and CLAUDE.md up to date.** Owner: Holt. STATE's header to 076o, open tasks
rewritten; CLAUDE.md's "Current state" gains the Action, contracts, the 3.5 default, the TypeScript
track and the side track. Accept: Rosa greps for "after decision 076k" and finds nothing; STATE is 60
lines or fewer. Rollback: `git revert`.

**T0.3 Record the model ruling.** Sunny rules; Holt records a decision with a "Touches" list:
`AGENTS.md` "Models and keys" and the "Build and safety" review rule. Accept: the two files and the
decision say the same thing. Until then, phase 2 tasks marked † are blocked.

**T0.4 Close the price question** (076k item 10). Sunny reads the per-model prices in the Nebius
console; Terry corrects `config/prices.json`; Amy gives the GO. Accept: the recomputed console-to-ledger
ratio is between 0.95 and 1.08. Rollback: revert the file; the ledger is never rewritten.

**T0.5 Decide what stays public on the side branch** (P4). Sunny's call: leave, or keep future run
outputs out of the public repository. No history rewrite either way (AGENTS.md forbids it).

### Phase 1: the missing evidence, subscription or under $1 of judging

**T1.1 Pre-register ablation A1.** New file `sidetrack/ab2-plan.md` on the side branch: arms, two
draws each, measures, the decision rule and the prediction from section 7. Accept: committed before
any A1 run; Amy confirms the commit time.

**T1.2 Add a no-lookup mode to the runner.** File: `scripts/track-c/review-case.ts`. Add
`--packet none`: do not write `affected.txt`, and drop the "affected.txt" line from `buildPrompt`.
Use a copy of the skill text with step 1's second command and its paragraph removed, selected by the
flag, so the lookup arm's prompt bytes do not change. Test: a unit test that `buildPrompt` for `none`
contains no "affected" and that the `full` prompt's sha256 is unchanged. Command:
`pnpm --filter @tracehound/analyzer exec vitest run test/track-c*.test.ts`. Rollback: delete the flag.

**T1.3 Keep the run records.** Same file: default `--out` under `~/Projects/tracehound-runs/dev/`,
and print a one-line summary (median cost, input tokens, wall time) that can be committed as counts.
Accept: after a 2-case smoke run the summary exists and has no PR title in it.

**T1.4 Exposure count.** New script `scripts/track-c/exposure.ts`: for each dev case, the committer
date of the fix commit and whether it is before 1 July 2026. Output counts only. Dev clones only;
git with explicit paths outside the held-out area. Accept: a table "exposed N, unexposed M" in the run folder.

**T1.5 Run A1-none, two Sonnet draws, then judge.** Runs on Sunny's subscription; judging by Terry
with bench-judge-v1, sequential, cap $0.10, approved by Holt. Accept: catches of 31 per draw, paired
counts against both lookup draws, and the decision line from T1.1 applied as written.

**T1.6 Fix deletion-only changes in the lookup** (S1). File:
`packages/analyzer/src/affected/affected.ts`, step 1. For each hunk with removed lines and no added
lines, treat the head line at the hunk's new-side start (and the line after it) as changed for the
purpose of finding the enclosing declaration; record such declarations with a `deletedLinesOnly: true`
field. Bump `AFFECTED_VERSION`. Test in `packages/analyzer/test/affected.test.ts`: a fixture where a
guard line is deleted inside an exported function with one caller in another file; expect the function
listed with that caller. Command: `pnpm --filter @tracehound/analyzer exec vitest run test/affected.test.ts`.
Rollback: revert the commit; the version string tells old packets from new.

**T1.7 Offline quote check.** New script `scripts/track-c/quote-check.ts`: for each comment in the
7 scored folders, extract every `path:line` and backticked code span, and check the span occurs within
3 lines of that line at the case head. Report per folder: comments failing the check, and how many of
those the judge had matched. No model. Accept: the table exists; adoption follows section 9's bar.

### Phase 2: product hardening (each is independent; † needs T0.3)

**T2.1 Retry transient failures in the client** (R2). File: `packages/analyzer/src/llm/client.ts`,
`chat()`. Retry up to 2 times on a thrown fetch error, HTTP 429, and HTTP 500 to 599, waiting
`Retry-After` if present, else 2 s then 8 s. Ledger one row per attempt; charge the upper bound only on
the last failed attempt when no response arrived. Requests stay byte-identical, so frozen prompts and
cache keys are untouched; it still needs a decision entry because run records change. Tests in
`packages/analyzer/test/llm-client*.test.ts` with a fake fetch: 503 then 200 gives one result and two
ledger rows; three failures throw. Command: `pnpm --filter @tracehound/analyzer test`. Rollback: a
`maxRetries: 0` option restores today's behaviour.

**T2.2 Tolerate a bad ledger line** (R10). File: `llm/ledger.ts`, `entries()`. Skip a line that
fails to parse, count it, and warn once on stderr. Test: a ledger whose last line is cut in half
still totals the complete rows. Rollback: revert.

**T2.3 Bump the Action pin** (R3). After the held-out tag, pin the example workflow and the repo's
own workflow to a commit that has the lenient parser. Run the install matrix and one dry run first.
Accept: a dry run on a fixture PR prints a review; the README's failure-rate line is re-measured or
marked "at the old pin". Rollback: restore the old sha.

**T2.4 Share the review loop** (R11). Move the request, retry and validation loop of `review.ts`
into one function that takes "build the user message for this chunk"; `review-ctx.ts` passes its own.
Accept: `packages/action/test/pool-verify-identity.test.ts` passes with its snapshot unchanged, and
both prompt hashes are unchanged. Rollback: revert.

**T2.5 Filter files before chunking** (R8). A default ignore list (lockfiles, `*.min.*`, `dist/`,
`*.snap`, files marked `linguist-generated`) applied in the Action before `chunkDiff`; skipped files
are named in the review body. Off by default until measured on dev by replay: catches unchanged,
cost reported. Rollback: the flag.

**T2.6 Do not repeat a review** (R7). In `runAction`, list earlier reviews on the PR, parse the
footer of each one from the bot, and drop any new finding whose path and line (within 3) match a
finding already posted for the same head or an ancestor. Test with a fake GitHub that returns one
earlier review. Rollback: an input `dedupe: off`.

**T2.7 Neutralise model text** (R13). Apply the escaping already used for contract text
(`escapeContractText`) to mentions and HTML in model bodies, leaving code spans intact; pass the
analyzer subprocesses an environment without `GITHUB_TOKEN`. Tests: the security fixtures of
section 10. Rollback: revert.

**T2.8 † A provider input for the Action** (R6). Inputs `provider` (`nebius`, `anthropic`,
`openai`) and one key per provider; the fix stage uses the same provider and the `model` input.
Structured outputs where the provider has them. This is a new reviewer version, not a change to
pool-verify-v1: it gets its own name, its own pre-registered dev comparison, and never replaces the
frozen row silently. Accept: mocked tests per provider; a dev replay with two draws. Rollback: the
default stays `nebius`.

**T2.9 † Ship the skill for users' own Claude Code.** A documented workflow that runs the skill
through Claude Code's official GitHub Action with the user's own credentials, and a local slash
command. No TraceHound-held token. Accept: a dry run in a scratch repository posts one review.
Rollback: remove the document.

**T2.10 Port the narrow typed check into contracts** (R5), behind a flag, after reading
`typed-narrow.ts`. Accept: on the 40 dev cases it finishes on at least 38 within 60 s each, and its
findings are listed for a hand check before the flag turns on.

### Phase 3: evidence for a public claim

**T3.1 Build E1** (fresh cases) and **T3.2 E2** (seeded cross-file) and **T3.3 E3** (clean), each
with a labelling note, two labellers, and a plan committed before the first review. **T3.4** Run the
best arm from phase 1 against the frozen A0 on E1, two draws each, both judges. Gate: a gain of 4 or
more of 40 that holds on unexposed cases. **T3.5** Publish the numbers whichever way they fall.

Later, each behind its own pre-registered test: an intent-against-diff check; looser finding rules
with explicit stay-silent rules; a one-hour probe of type-aware lint output as facts; a log of A/B
outcomes in tracehound-brain (counts only).

---

## 12. Five refinement passes

**Pass 1, repository correctness.** Weakness: I first took the handover's "fixes only on the side
branch" at face value. Counter-evidence: `review.ts:177` on `main` and its absence at the pin.
Revised: R3 is about a stale pin, and R4 states what is still open. Also corrected: pending scores,
tree equality, test counts. Still uncertain: GitHub's diff-size limit; whether any test already drives
real subprocesses (I grepped, I did not read every test file).

**Pass 2, research validity.** Weakness: the fetch tool summarises pages, and every vendor number is
self-reported. Counter-evidence found against the handover: Bugbot's 8-pass voting is sourced.
Counter-evidence against my own draft: Augment's F1 appears as 53.8% in its March post and 59% in a
search excerpt of its launch post, so I report a band and no ranking. Revised: vendor comparisons are
limited to mechanism and price. Still uncertain: models behind CodeRabbit and Greptile; the Martian
leaderboard; OpenAI model availability.

**Pass 3, architecture and the value of graphs.** Weakness: my draft made the symbol lookup the
product's differentiator. The evidence does not support that: Haiku with the lookup gained nothing,
Sonnet's gain may come from file access alone, and SWE-PRBench found added context hurting single
calls. Counter-evidence for graphs: two vendors sell one as their core, but they do not publish an
ablation. Revised: A1-none runs before anything is built on the lookup; an arm that offers the lookup
as a tool is added; the component graph stays out of the reviewer unless an arm clears the bar.
Still uncertain: everything about cross-file value, until E2 exists.

**Pass 4, evaluation, economics, viability.** Weakness: I was treating dev as if it could rank arms.
It cannot: 31 cases, a +5 gap at p 0.18, 9 clean cases, and at least 10 looks. The Sonnet agent is
not a registered held-out row, so no clean confirmation of it is coming. Revised: E1 becomes the gate
for any new arm; E3 is added for false positives. On economics, my draft spent effort on routing and
caching; with cost at $0.05 to $0.12 against prices of $1 a review and up, those are deferred behind
quality. Still uncertain: true API cost of the agent (the records are gone); the Haiku price step.

**Pass 5, feasibility and consistency.** Weakness: several draft tasks edited files I do not own or
that are frozen or pinned. Revised: the roadmap is a note to Holt; nothing touches the frozen
reviewer before the tag; T0.1 checks the P1 pins; provider work is a new reviewer version, not an
edit to pool-verify-v1. Checked against the project's rule on lost levers: no model verifier and no
voting are proposed; the quote check is flagged as touching 065 with its reason. Checked against
076m: no API spend before held-out scoring. Fable access ends 17 Nov, so each task names its files,
command, acceptance test and rollback for a cheaper model. Still uncertain: whether the team has the
hours for phase 3; T2.10 depends on a file I did not read.

---

## 13. Coverage log

| Area | Depth | Gap and reason |
|---|---|---|
| `AGENTS.md`, `.agents/STATE.md` | Read in full | |
| `CLAUDE.md` | As supplied to the session | |
| `docs/decisions.md` | 076g to 076o in full; headings for 047 to 076f | 001 to 075 bodies not read: relied on CLAUDE.md's summaries |
| `.agents/LOG.md`, `docs/budget.md` | Last 40 and 30 lines | Earlier history not read |
| `README.md` | The lines the handover cites | Rest not read |
| `packages/action/src` | `cli`, `reviewer`, `pipeline`, `action`, `contracts`, `fix`, `usage`, `fix-prompt` in full | `learn.ts`: header only. `data.ts`: first 120 lines |
| `packages/analyzer/src/review` | `review`, `prompt`, `prompt-ctx`, `cli`, `diff`, `names`, `context`, `review-ctx`, `verify-ctx`, `verify-ctx-prompt` in full | `verify-context.ts`: header. `recall/`, `missed`, `verify`, `verify-evidence`, the other CLIs: not read (lost or unused levers) |
| `packages/analyzer/src/llm` | `client`, `budget`, `ledger`, `cache`, `prices`, `transport`, `provider`, `models` in full | `anthropic.ts`: first 80 lines |
| `packages/analyzer/src/contracts` | `contracts.ts` in full; `typed.ts` first 80 lines and its limits; `keyed.ts` header | `dependents`, `findings`, `shapes`, `trees`, `projects`: not read |
| Graph analyzer, changes, impact, finder, agent, harness, naming, bench, learn, gate | Import check only | Not read: off the review path; time |
| `viewer/` | Tests run only | Source not read |
| `scripts/` | Names only | Not read; `scripts/testset` is pinned by the running P1 scheduler |
| Workflows | `tracehound-review`, `ci`, `action-install-matrix` | `reviewbench-*`: not read |
| Tests | Ran typecheck (clean), `pnpm test` (analyzer 1,075 passed, 6 skipped, 1 failed), the failing file alone (58 of 58), action (411 passed), viewer (225 passed) | Most analyzer test files not read |
| Side branch | `SKILL.md`, `affected.ts`, `review-case.ts`, `ab1-plan.md` in full | `typed-narrow.ts`, `score.ts`, tests: not read. Review outputs: one file's keys only |
| `tracehound-testset/run/out` | Listed; read `bunny.076o.md`, `bunny.076n.md`, `codex.076j.md`, the dev-export note | The export JSON was not opened |
| **Firewall** | No held-out file opened, listed, grepped or diffed. No git in the test-set repo. No read of the four forks. The private benchmark and evaluation folders untouched. `.tracehound/cache` and `.env` not opened | Held-out data, Cal.com detail and per-comment scores are coverage gaps by rule |
| Web | Anthropic pricing and models; OpenAI pricing and models; Cursor blog and docs; CodeRabbit plans; Greptile pricing and docs index; Augment blog; Anthropic code review post; Claude Code legal page; SWE-PRBench abstract; Martian README | Read as summaries. Not read: Copilot, Graphite, Greptile blog, the Martian leaderboard (empty) |

---

## 14. Not verified

- Every side-track cost, token, memory and timing figure in the handover (the records are gone).
- Whether the dev bugs' fixes are in any model's training data (S3).
- That A0's 12 of 31 or Sonnet's 17 of 31 hold on unseen cases.
- GitHub's size limit for the diff media type.
- OpenAI and Anthropic model availability and rate limits on Sunny's accounts; OpenAI structured-output behaviour.
- Nebius prices; the cause of the 15% ledger gap.
- All vendor quality numbers; the models behind CodeRabbit and Greptile; Greptile's and CodeRabbit's internals; Copilot and Graphite; Gemini's market figures.
- Whether dev or held-out labels carry a severity.
- That no existing test already runs the Action through real subprocesses.
- Behaviour of code I did not read (section 13).

## 15. Needed from Sunny

1. A ruling on whether "Nemotron only" is lifted for product code (T0.3).
2. The held-out result when it exists, and the CodeRabbit and Greptile dev rows after P1 closes (about 17 Nov).
3. Nebius per-model prices from the console, and current Nebius, Anthropic and OpenAI balances and any limits.
4. Any surviving copy of the side track's run records.
5. Gemini's full report.
6. Whether labels have severities, and whether the fix-commit dates of the dev cases may be computed from the dev clones.
7. A decision on the public side-branch outputs (T0.5).

No longer needed: the dev export (it is at `tracehound-testset/run/out/dev-export-for-sidetrack.json`)
and Terry's scores (decision 076o).
