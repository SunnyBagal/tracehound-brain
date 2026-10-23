caffeinate -i bun src/relay.ts start tracehound briefs/069b-v2-intervals.md
relay: tracehound/034 builder running in /Users/sunny/Projects/TraceHound (run 29, cycle 1 of 6)
relay: tracehound/034 reviewer running
# Review tracehound/034: STOP_FOR_SUNNY -> BLOCKED

Run 29, cycle 1 of 6. Claude spend this run $2.99 of $30.00. Reviewer attempt 1, session 917b4a30-0ca2-496f-b0a4-7ebedb5f5751.

## HARD STOPS (enforced by relay)
- reviewer verdict is STOP_FOR_SUNNY
- reviewer has 2 question(s) for Sunny

# Review: lane cycle 034 (run 29, cycle 1), 069b: intervals for the v2 gaps

**In short:** the job is done for $0. Draft PR #60 (head `aec07e3`, CI green) is ready to merge, so I'm stopping for you. The plan was committed before the script existed. The script does what the plan says. Every input count I could check matches the published totals.

### What the result says
These are the sentences the pre-registered wording rule allows:
- **All 50 PRs, both judges:** v2 is behind Augment, statistically tied with Cursor and Greptile, and ahead of Copilot and CodeRabbit.
- **Cal.com (10 PRs):** statistically tied with every row except CodeRabbit, where it is ahead.
- **Against 3 Super on all 50:** ahead under v1, tied under x1.
- **Rank:** "3rd of 6" may be used only as a point estimate.

### What I checked
- **Scope.** The builder's 5 commits (`5fb8cdc..aec07e3`) touch exactly 6 files, all in may-touch: the plan, the result, the prompt, decisions.md (appended after 069, which is the last decision), `scripts/bench/intervals.ts` and its test. `results.md`, the README, `viewer/`, `packages/*/src`, `config/` and `.github/` are untouched.
- **Timing.** The plan `5e16e2b` was committed at 16:07:40Z. The script `36ce0f3` was committed at 16:09:13Z, the same time the builder gives for the first bootstrap, so the plan existed before any code to compute it. The push time comes from the session.
- **The script matches the plan.**
  - Paired resampling uses the same index `j` on both rows (`intervals.ts:74-76`).
  - Pairing is by `repo#goldenIndex`, and the script refuses rows with different PR sets.
  - A JUDGE_FAILED PR on either side is dropped from both and counted.
  - Percentiles use R type 7. Share > 0 is strict. There are 10,000 resamples, seed 69, with a fresh generator per gap.
  - `sentence()` uses only the primary interval.
  - The reproduction gate is ≤ 0.1 on every published bound.
  - The tests cover a known gap, identical rows giving exactly 0, seed determinism, the JUDGE_FAILED drop, pairing by key, and the wording rule.
- **Counts.** The all-50 tool rows in `v2-intervals.md` (for example Augment 56/16/41 and 56/17/41, Copilot 43/104/54 and 50/91/47) match `results.md:116-134`. 3 Super row (i) matches too (40/57/57 and 45/45/52). v2's counts (47/36/50 and 49/31/48) match 069. I recomputed the Cal.com observed gaps from the TP/FP/FN columns: Augment −16.0, CodeRabbit +20.4 and 3 Super −1.2 under v1, and +0.2 under x1. They match.
- **Labels.** Every allowed sentence follows the table under the rule. For example, Cal.com v1 Augment [−30.9, +3.5] is tied even though its share > 0 is 0.044. All-50 x1 Augment [−19.8, −0.3] is behind.
- **Fixed refs and spend.** The ledger delta is $0.00000, which fits a no-call job. eval-freeze-2 is still `acefcf9`, and the release is still a DRAFT. The tree is clean and nothing was merged.

### Things that are fine but worth knowing
- **Secondary method skipped.** The audit's interval wasn't reproduced to the 0.1 bar, so the secondary interval was skipped as pre-registered. That is the right call under the brief's rule. The report's summary ("six inflated bounds 0.1–0.2 off") is a little loose. Bounds 0.1 off pass the script's ≤ 0.1 bar. The failures are the two 0.2 bounds plus x1 Greptile (0.4) and x1 Copilot (0.2). `v2-intervals.md:17-24` says this correctly.
- **"Ahead of CodeRabbit" on Cal.com rests on a 10-cluster bootstrap.** That likely under-covers, and the x1 lower bound is +1.4. The doc says so plainly. Any public sentence should carry that caveat or use the all-50 result instead.
- **One sentence depends on the dev run chosen.** With dev 8 instead of 7r, all-50 x1 "behind Augment" becomes tied ([−19.2, +1.3]). It is recorded as report-only, as planned.
- **"Ahead of 3 Super" is judge-specific.** It holds only under v1, and the all-50 dev PRs are v2's selection set. Both points are stated.
- **`freezeCheck069` now fails because of the new file.** This is disclosed, and no further Cal.com run is allowed anyway.

### Conflict with the published page
`results.md:8` still says TraceHound is "4th of 6" on all 50. That was 3 Super's row (i), and it stays true for that row. Any update to the results page now has to use these intervals and the wording rule.

### Losses and limits (all recorded in the doc)
- v2 is behind Augment on all 50 under both judges.
- v2 has no interval-backed lead over Cursor or Greptile anywhere.
- The intervals don't cover run-to-run spread or judge error, and there is no correction for testing 24 gaps.

VERDICT: STOP_FOR_SUNNY

## Checked claims
- The plan 5e16e2b was pushed at 16:07:42Z, before the first bootstrap at 16:09:13Z
  - checked: git log --format='%h %ad %s' --date=iso-strict 5fb8cdc..HEAD
  - result: The plan commit is at 21:37:40+05:30 (16:07:40Z). The script commit 36ce0f3 is at 21:39:13+05:30 (16:09:13Z), so the plan came before any bootstrap code. The push time comes from the session.
- The primary method is a PR-level paired bootstrap, 10,000 resamples, seed 69, same PRs both sides, JUDGE_FAILED dropped from both
  - checked: Read scripts/bench/intervals.ts:56-84 and packages/analyzer/test/intervals.test.ts
  - result: Confirmed. The same index j is drawn for both rows. pair() matches by repo#goldenIndex, refuses different sets and drops a PR failed on either side. Tests cover the known gap, determinism, the drop, pairing and the wording rule.
- All-50 inputs: v2 47/36/50 (v1), 49/31/48 (x1); the tool and 3 Super row (i) counts as in the table
  - checked: Read docs/bench/v2-intervals.md:45-72 against docs/bench/results.md:114-134
  - result: Confirmed. Augment, Cursor, Greptile, CodeRabbit, Copilot and row (i) match under both judges, and the F values match.
- Cal.com observed gaps (e.g. v1 Augment −16.0, CodeRabbit +20.4, 3 Super −1.2; x1 3 Super +0.2)
  - checked: Recomputed F = 2TP/(2TP+FP+FN) from the TP/FP/FN columns in v2-intervals.md:30-41
  - result: Confirmed. 57.1−73.2, 57.1−36.7, 57.1−58.3, and 58.5−58.3 under x1.
- Only the may-touch files changed; results.md, the README and viewer/ are untouched
  - checked: git diff --stat 5fb8cdc..HEAD
  - result: Confirmed. There are 6 files, all in may-touch, and decisions.md is append-only after 069.
- $0 spent, no model calls
  - checked: Relay ledger facts
  - result: Confirmed. The run delta is $0.00000.

## Claims not backed by facts
- The push time 16:07:42Z and the first bootstrap at 16:09:13Z: from the session only. I can see only commit times, which agree.
- The per-PR score files existing and summing to the published totals, and 0 JUDGE_FAILED: the files are under tracehound-bench, outside my tools. The resulting sums match the published rows.
- The bootstrap interval values themselves weren't recomputed. That needs the per-PR records.
- The reproduction-check numbers (the audit's bounds against the recomputed ones): from the session only.
- The local test counts (913/237/188): from the session only, though CI is green.
- That no Cal.com text was opened: can't be checked from the repo. The docs contain counts only.
- The brain inbox commit 3b22209: outside my tools.

## Unrequested changes
- The plan adds a reproduction bar for the secondary method (≤ 0.1 on every published bound, shares compared but not deciding). This expands on the brief's '0.1' rule. It is pre-registered and stricter, not looser.

## Questions for Sunny
- PR #60 (069b intervals, draft, head aec07e3, CI green, docs and a bench script only) is ready. Will you mark it ready and merge it with 'Create a merge commit'? Marking it ready may trigger the dogfood review on the Action's key.
- The results page (results.md:8 still says '4th of 6' for 3 Super row (i)), the README and the viewer need 069 plus these intervals and the wording rule. Should this lane do it as decision 070 at $0 (the proposed prompt below), or should it go to the Codex lane or #54 (judges' README)? The plan says the Grok audit runs on 24–25 Oct.

## Next prompt (pending: 034/next-prompt.md)
Proposed for Sunny (not sent). Use it only if Sunny gives the results-page update to this lane after #59 and #60 are merged.

# 070: put the v2 Cal.com result and its intervals on the results page

Folder: ~/Projects/TraceHound. Fresh session. Decision 070. Nebius: $0 (no model calls).

## Step 0 (if any check fails, STOP AND REPORT)
- git fetch origin --tags. git status --porcelain prints nothing.
- Switch to main and fast-forward with merge --ff-only to origin/main. origin/main contains the merges of #59 and #60.
- grep -c '^## 070' docs/decisions.md prints 0.
- The GIT_* env check prints nothing: env | grep -E '^GIT_(DIR|WORK_TREE|INDEX_FILE|OBJECT_DIRECTORY|ALTERNATE_OBJECT_DIRECTORIES|COMMON_DIR|NAMESPACE|PREFIX|CONFIG)'. Repeat it before every gate.

## Steps (one commit each)
1. Create branch build/070-results-v2. Save this prompt verbatim to docs/prompts/070-results-v2.md. Commit.
2. Update docs/bench/results.md using only counts and intervals already published in docs/bench/v2-calcom.md and docs/bench/v2-intervals.md:
   - the Cal.com run 4 headline under both judges;
   - an all-50 v2 table, with the primary 95% interval for each gap beside it;
   - the limits from both docs, word for word.

   Use only the sentences in v2-intervals.md's 'The sentences the rule allows' section:
   - "ahead" or "behind" only where the primary interval excludes 0;
   - otherwise "statistically tied";
   - ranks only as "a point estimate".

   State plainly that v2's Cal.com F (57.1%) is inside 3 Super's spread and below its run 1, and that the gain is precision with a loss of recall. Never write that 3.5 improved Cal.com F.

   Keep the existing 3 Super rows (i) and (ii) and their '4th of 6' text, labelled as 3 Super. Add no new numbers.
3. Update the viewer's results data and its test to match results.md. Change only what the new rows need.
4. Update README.md's results claims to match. Claim nothing the two docs don't state.
5. Append decision 070 to docs/decisions.md.
6. Run pnpm test, pnpm typecheck and the viewer build as separate commands, and read each result. Commit, push, and open a draft PR titled '070: v2 result and intervals on the results page (decision 070)'. Wait for CI and make one fix attempt if it's red. Never merge.

## Rules
- Cal.com is blind: open nothing under ~/Projects/tracehound-bench/test/. Use only counts already published in docs/bench/.
- May touch: docs/bench/results.md, README.md, the viewer's results data and its test, docs/decisions.md (append only), docs/prompts/070-results-v2.md.
- Don't touch: scripts/bench/, packages/*/src, config/, .github/, AGENTS.md, pnpm-lock.yaml, docs/bench/v2-calcom*.md, docs/bench/v2-intervals*.md, or anything in ~/Projects/tracehound-ops/.
- No Nebius calls and no network calls beyond git and gh.
- No tag operations, no history rewriting and no destructive git commands. Stage by explicit path.
- Nothing else runs on the machine during a gate.

## STOP AND REPORT
- step 0 results;
- the diff stat;
- each changed number or sentence with its source file:line;
- the gate results;
- the PR number and head sha;
- anything not done.
relay: tracehound is BLOCKED
  - reviewer verdict is STOP_FOR_SUNNY
  - reviewer has 2 question(s) for Sunny