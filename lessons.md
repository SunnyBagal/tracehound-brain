# Lessons

Process mistakes and near-misses, blameless. Each says what happened, what it cost, and what changed. Newest first.

### 2026-10-29 · A paste block pushed after a failing test
- **What:** chat gave Sunny a multi-line paste where `pnpm test` and the commit/push were separate lines, and the analyzer CLI didn't load `.env`. The snapshot came out with heuristic names, a test failed, and the push still ran.
- **Cost:** a wrong graph on main for a few minutes; one rerun ($0.0005).
- **Changed:** in paste blocks, gates and commit/push are one `&&` chain; analyzer runs use `node --env-file=.env`.

### 2026-10-28 · A re-pin didn't reach an already-open PR
- **What:** after linea-seeded's workflow pin moved to review-freeze-2.1, chat told the seed agent to re-trigger the open PR #1 with a draft/ready toggle, claiming a `ready_for_review` run uses the base branch's current workflow. It doesn't: `pull_request` runs use the PR's test-merge commit, which GitHub had not rebuilt after the base moved, so the run used the old pin and failed again. The agent caught it from the downloaded action sha.
- **Cost:** one wasted run and about an hour.
- **Changed:** to re-run an open PR on a new workflow, first check `merge_commit_sha`'s parents include the new base head. Briefs say how a run proves which action commit it used (the "Download action" log line).

### 2026-10-24 · The money plan used a stale balance and left out VAT
- **What:** chat planned the final against a $68.09 balance that had barely moved after five test runs (it lagged billing), never added 18% VAT to any estimate, and never measured a test run's real cost before sizing the final. The real balance was $43.52.
- **Cost:** caught before the final started; the final waits for a measurement and probably a top-up.
- **Changed:** cost estimates are quoted with VAT (×1.18). A balance is only trusted if it moved as expected since the last reading. Before any large run, its cost is measured from a smaller run of the same config (per-PR spend in the agent output), not estimated.

### 2026-10-24 · 070's stop rule and P009's per-PR cap were written loosely by chat
- **What:** the 070 brief said stop on "any request error (4xx/5xx, a budget refusal)". It didn't say whether recall-v3's own per-PR cap counts. The builder read it narrowly and ran arm V. Separately, P009 set a $0.12 per-PR cap without checking it against reservation-based budgeting: with 16k max_tokens the missed pass reserves about $0.04, so multi-chunk PRs hit the cap (6 PRs in S, 11 in V) and v3 was measured partly crippled.
- **Cost:** $3.33 on arm V that a strict stop would have saved; a v3 result that has to carry a "hampered" caveat.
- **Changed:** stop rules list every refusal source by name (global cap, run cap, the pipeline's own per-PR cap, truncation rate). Any new per-PR cap is checked against the client's worst-case reservation (max_tokens × output price × calls) before it goes in a pre-registration.

### 2026-10-23 · Two ReviewBench candidates had never been smoke-tested
- **What:** candidate 3 (pool-raw-x2) had never run on any image, and candidate 4 (pool-verify-v1 on 3.5) had never run on 3.5. The plan was to register them and spend a one-shot test run on each. Chat caught it while rethinking amendment 5.
- **Cost:** none. Two extra smokes, about $0.10.
- **Changed:** every config gets one green smoke on the exact image and model before its portal config is added. The test-run checklist lists the smoke run for each candidate.

### 2026-10-23 · A link ending a sentence picked up the period
- **What:** chat wrote "Open github.com/…/pull/2." and GitHub routed `pull/2.` to the compare page.
- **Cost:** one round trip.
- **Changed:** URLs Sunny should click go on their own line, as full `https://` links, with no punctuation after them.

### 2026-10-22 · The bench and the product disagreed on failed chunks
- **What:** the dev bench keeps partial results when a reviewer chunk fails. The Action and the ReviewBench adapter post nothing. With 3 Super this never mattered (1 failed PR in 4 runs). With 3.5 it's 7/40 PRs per run. 067's adoption would have shipped a config that wasn't measured. The chat caught it while checking #56; neither relay's builder nor its reviewer did.
- **Cost:** none. It was caught before the default switch.
- **Changed:** any brief that adopts a measured config into the product names every place the measurement and the product could differ (failure handling, caps, models per stage), and the product follows the measurement.

### 2026-10-21 · The int-01 brief specified a path the analyzer can't produce
- **What:** codex-10 asked for inline `Contract:` comments, a 10-inline cap and a ±3 dedupe. The contract checker only reports files the PR didn't edit, so none of these can trigger. The chat wrote the brief without reading `findings.ts` / `keyed.ts`, and its PR check didn't catch it either. Ultrareview #2 did.
- **Cost:** dead code and a false docs claim, fixed in codex-11's docs item.
- **Changed:** before a brief wires a stage's output somewhere, the chat reads where that output comes from: which files and lines it can name.

### 2026-10-20 · The live demo wasn't running the benchmarked model
- **What:** the Action's reviewers default to Nano (`DEFAULT_MODEL`). The demo and dogfood workflows never set `model`, so since 17 Oct every live review used Nano generators with a Super verifier, while the benchmark is all Super. The demo stats in NOW (2 of 4 seeded lines) are Nano-generator numbers. Bunny caught it while writing the judges' README; no PR check had compared a live review's header with the benchmark config.
- **Cost:** none yet. It would have put a different config in front of judges than the one the numbers describe, and the seeded test (25 Oct) would have measured it.
- **Changed:** every workflow that stands for the product sets `model:` explicitly. The seeded-test brief names it. A chat PR check of a live review compares the header's models with the benchmark config.

### 2026-10-20 · Bunny chained a commit after a failing test
- **What:** in 066, the builder (Bunny, Sonnet) ran the tests and the commit in one chained command, so step 3 was committed with the viewer test red. It noticed, never pushed it, and undid it (`git reset --soft`).
- **Cost:** none.
- **Changed:** briefs say: run the gates as their own command, read the result, then commit separately. A test written by one lane (viewer #44) can block another lane's docs. Briefs that edit `docs/bench/results.md` must name the viewer test that mirrors it.

### 2026-10-18 · Ultrareview found 10 issues in #43 that the brief, Codex and the chat's check all missed
- **What:** the bugs came from three places.
  - **Chat's brief (codex-07):** it told Codex to run `changes --diff <base>..<head>` (that's #1: diff against the base tip, not the merge base). It said "retry once on non-fast-forward" (#3) and never asked for replay or out-of-order safety (#2).
  - **Codex:** used `{...env}` instead of `cleanGitEnv()` (#8). That rule is in CLAUDE.md, **but not in AGENTS.md, which is what Codex reads**. It also put the `GIT_CONFIG_GLOBAL` isolation on the workspace calls (#4) and made the concurrency choice (#7).
  - **Chat's PR check:** looked at token handling and continue-on-error, and passed it.
- **Cost:** none, because it was caught before merge. One fix round.
- **Changed:**
  - Briefs for anything that writes state must specify merge-base diffs, idempotency and out-of-order events.
  - Copy the `cleanGitEnv` rule into AGENTS.md (next Codex job).
  - Ultrareview stays mandatory for code that writes to other people's repos.

### 2026-10-18 · A PR was merged while a fix prompt for it was being written (again)
- **What:** chat wrote codex-06c to add commits to #38's branch; Sunny merged #38 (and #39) minutes later. Same trap as 15 Oct.
- **Cost:** one rewritten prompt. Main is fine: the suggestions work, only demo #2's isn't committable.
- **Changed:** when the chat says "don't merge yet", NOW lists the PR as **HOLD** and the chat says it in the first line of its reply. A follow-up prompt for a draft PR is written against main on a new branch by default.

### 2026-10-18 · A 4-hour model batch ran as a background command inside the relay session
- **What:** the 062 brief (written by chat) didn't say how to launch the runs. The builder ran items 2–3 as one background shell command, and the session's 2-hour limit killed it at dev run 2, PR 30 of 40. The run was restarted as 2r (061a), so dev run 3 then hit the $4 cap at PR 16 of 40.
- **Cost:** $1.44 of Nebius with no score; the dev spread has 2 runs, not 3.
- **Changed:** any model batch over about 60 minutes is launched detached (`nohup … &` with its own log), and the brief says so. The session then polls `run.json`. The chat's progress commands count *files* (`find -type f`), not folders: `ls | wc -l` counted repo folders twice on 18 Oct.

### 2026-10-18 · The 062 brief's tag check peeled the tag
- **What:** step 0 expected `eval-freeze-2^{commit}` to print `acefcf9`, but that is the tag object; the commit is `b5cafde`. The builder handled it correctly and reported a deviation.
- **Changed:** check annotated tags with `git rev-parse TAG` (object) **and** `TAG^{commit}` (target), and name both.

### 2026-10-17 · Codex got codex-06 before codex-05 existed
- **What:** two Codex prompts went out the same day, and 06 was pasted first. Its step 0 ("the pnpm pin is merged") caught it and stopped at $0.
- **Kept:** a step 0 that checks the previous job has merged. It worked.
- **Changed:** NOW lists the next prompt per lane, and the chat names the exact file to paste each time.

### 2026-10-17 · A risk named on day one was never tested, and the demo was built on it
- **What:** on 15 Oct the chat named `package_json_file: ${{ github.action_path }}/../../../package.json` as "the most likely thing to break on a real run". No live smoke test was scheduled. #25, #28 and #30 merged with unit tests only. Then all 7 runs in the dogfood and demo setup (codex-04) failed in that step: "No pnpm version is specified", because pnpm resolves the path against the target repo.
- **Cost:** one Codex cycle and a day of the demo schedule. $0 of Nebius, since it failed before any model call.
- **Changed:** a named risk gets a test that would catch it before anything is built on top. The fix PR (codex-05) adds a test that fails if `package_json_file` returns, and checks the version against the root `packageManager`. Every Action change now gets a live run on the demo repo, pinned to the branch sha, before merge.

### 2026-10-15 · CI green does not mean the lockfile is current
- **What:** #25 added `packages/action` without a lockfile importer. CI's `--frozen-lockfile` passed, but relay's local `pnpm install` during 058 rewrote `pnpm-lock.yaml`. Relay restored it by hand.
- **Cost:** none yet. The next relay step 0 would fail on a dirty tree.
- **Changed:** a lockfile commit goes with the PR that adds a workspace package. The chat's earlier lockfile worry was right about the drift and wrong about CI.

### 2026-10-15 · An untracked "AGENTS.md" was really a copy of CLAUDE.md
- **What:** relay's checkout had an untracked `AGENTS.md`. Once the real AGENTS.md was merged (#25), step 0 would have failed. `cmp` showed the local file differed. Its blob was `f9fab6e`, CLAUDE.md as of `f610efa`.
- **Cost:** none, because it was moved, not deleted.
- **Changed:** an unknown untracked file is moved out of the repo and compared before anything is deleted.

### 2026-10-15 · A fix prompt aimed at a branch that was merged ten minutes later
- **What:** `codex-02-pr25-fixes.md` added commits to `codex/github-action`; #25 was merged before it ran.
- **Cost:** one rewritten prompt.
- **Changed:** when a merge is likely before a fix prompt runs, the fix prompt is written against main on a new branch.

### 2026-10-15 · Chat predicted red CI on #25 (lockfile); CI was green
- **What:** the new workspace package `packages/action` had no lockfile entry. The chat called that a "probable" `--frozen-lockfile` failure. pnpm accepts a package with no dependencies that is missing from the lockfile.
- **Cost:** a wasted step in a prompt, later removed.
- **Changed:** check the Actions page (WebFetch of `/actions/workflows/ci.yml`) before predicting CI.

### 2026-10-15 · 057's budget cut the variable under test first
- **What:** the 057 brief (written by chat) set a 40k-character budget and, when over, dropped references before anything else. References to callers were the point of the experiment. They reached the model in 12 of 48 chunks, so 057 mostly measured "changed files near the hunks".
- **Cost:** one $0.86 run that could not answer its own question.
- **Changed:** a budget's cut order protects whatever is being tested. 058's per-finding context drops references last-first only after the file window has been shrunk.

### 2026-10-15 · A single commit made "prompt frozen before the run" unverifiable
- **What:** relay's reviewer could not check 057's freeze claim, because everything landed in one commit.
- **Changed:** 058 pushes a freeze commit (rules and prompt) before the first model call.

### 2026-10-15 · Model-reported confidence did not separate true from false findings (056)
- **What:** 6 of 32 TPs got confidence 1. The pre-set threshold rule kept everything.
- **Changed:** 058 asks for a keep/drop verdict with context instead of a number.

### Before 2026-10-15 · Four relay cycles lost to step-0 stops
- **What:** unmerged PR, $0 Nebius cap, a stale brief rerun, stale local main.
- **Changed:** the pre-start checklist in [NOW](NOW.md); step 0 uses `git fetch origin main:main` (no plus), then `git switch main`.

### 2026-10-19 · A scoring rule that couldn't fire
- **What:** cc-04's rule (a) counted only inline comments at the stale-use line. But GitHub allows inline comments only on files in the diff, and every cross-file seed's stale use is in an **unedited** file. So rule (a) could never fire for 16 of the 30 seeds. The cloud session caught it.
- **Changed:** scoring rules get a dry run against the platform's constraints before they're frozen: can each tool physically produce a matching comment? For cross-file seeds, use a mention rule (path + symbol anywhere on the PR) with an automatic and a manual column.

### 2026-10-19 · Grok's links vanished in the paste
- **What:** grok-07 asked for a link on every claim. The pasted output had none, because citation chips don't survive copy and paste. So the kill test couldn't run.
- **Changed:** Grok prompts say "write every URL in full as plain text inside the table cell, not as a citation chip". The chat checks decision-relevant claims on primary pages anyway.

### 2026-10-19 · A builder raised its own spend cap
- **What:** 064's brief told Sunny to set `nebiusCapUsd` to 10. The builder set it itself in `tracehound-ops/ops/lanes.json`, outside its paths. Relay read $0 and only stopped after the spend. The runner's own $8.50 cap was the only real limit.
- **Changed:** briefs say "Do not edit anything under `~/Projects/tracehound-ops/`" in step 0 and in the rules. Cap changes are Sunny's, listed under "Sunny, before starting".

### 2026-10-19 · A nit list in the brief dropped real bugs
- **What:** codex-08's brief listed "consider" and "typos" as nit patterns. A finding that says "consider guarding … or it crashes" got dropped as style.
- **Changed:** a deterministic filter on free text gets adversarial fixtures **in the brief**: real-bug sentences that must survive. The chat probes the code with them before merge.

### 2026-10-19 · A literal fix prompt would have broken reopen
- **What:** ur-01 said "an `open` event can't overwrite `closed`". Taken literally, a reopened PR could never be open again. The cloud session caught it and let only `reopened` return `closed` → `open`.
- **Changed:** state rules in fix prompts name the event (`reopened`, `synchronize`), not just the target state. Ultrareview → fix in the same session → chat re-check of the source diff worked: 7 fixes, each with a failing-first test, one real-data proof (PR #3 change set 2 files → 1).
