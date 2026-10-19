# 065 run 2 output (relay 029, pasted by Sunny 19 Oct 16:06). Checked by chat: all 40 table rows and the 5-run means recompute; freeze diff empty; a loss.

caffeinate -i bun src/relay.ts start tracehound briefs/065-evidence-gate.md
relay: tracehound/029 builder running in /Users/sunny/Projects/TraceHound (run 25, cycle 1 of 6)
relay: tracehound/029 reviewer running
# Review tracehound/029: STOP_FOR_SUNNY -> BLOCKED

Run 25, cycle 1 of 6. Claude spend this run $6.22 of $30.00. Reviewer attempt 1, session 653b713e-a37f-47e8-a84c-d78a4852aa84.

## HARD STOPS (enforced by relay)
- reviewer verdict is STOP_FOR_SUNNY
- reviewer has 2 question(s) for Sunny

# Review: lane cycle 029 (run 25, cycle 1), decision 065, evidence verifier + deterministic gate (dev only)

**In short:** 065 is a loss and the builder recorded it as one. Neither A (gate only) nor B (the new verifier plus the gate) passes any of its pre-registered conditions, so pool-verify-v1 stays. Draft PR #48 has head `d2e5a6b` and CI passed on the first try. The dogfood review skipped it because it's a draft. I'm stopping because the PR is ready for you to merge, and because the lane has no next job queued.

### What I checked
- **The numbers add up.**
  - Every row I checked satisfies comments = TP+FP+dup+low and TP+FN = 74.
  - The means recompute:
    - B v1 F: 191.3/5 = 38.3, P 37.2, x1 F 45.7.
    - A v1 F: 208.2/5 = 41.6, P 40.3.
  - The verifier counts reconcile. keep + drop + VERIFY_FAILED equals the findings in every run (sums 525 + 308 + 29 = 862), and retries add up to 140.
  - The pipeline counts reconcile:
    - B: 525 kept plus 29 VERIFY_FAILED is 554 into the gate, minus 80 gate drops, leaves 474. That equals the sum of B's comment column.
    - B-all: 554 − 70 = 484, which matches.
    - A: 478 − 11 = 467, which matches.
- **Adoption was applied as written.** The plan's rules match the brief word for word. A fails all three conditions (P 40.3 vs 40.5, F 41.6 vs 42.3, x1 F 50.4 vs 51.3). B fails all three (−4.0 F, −3.3 P, x1 45.7 vs 51.3). The cap fallback was not needed.
- **The freeze held.**
  - The plan commit `9bfb3eb` is dated 08:41:40Z, and the driver `46d12d8` 08:46:03Z. Both are before the stated first call at 08:46:09Z.
  - `git diff 9bfb3eb d2e5a6b -- packages/analyzer/src/review/ docs/bench/evidence-plan.md` is empty, so neither the prompt nor the plan changed after the freeze.
- **Scope.** Relay's 25-file diff includes your merges of #45 and #41. The builder's own diff, `9a81505..d2e5a6b`, is 12 files and stays inside may-touch:
  - bin.ts gained exactly one line, and CLAUDE.md one line;
  - decisions.md is append only.
  - A diff over gate/, contracts/, packages/action, .github, viewer, config, pnpm-lock.yaml, docs/freeze.md and docs/eval is empty.
- **The gate CLI** (`gate/cli.ts:64-71`) has `--repo`, `--base`, `--require-evidence` and `--min-severity` under those names, so variants A and B used real flags.
- **Spend.** The verifier cost $2.5640 and judging $0.0871, $2.6511 in total. That equals the ledger delta of $2.65107, under the $5 cap and both sub-caps.
- **Fixed refs.** eval-freeze-2 is still `acefcf9`, the release is still a DRAFT, the working tree is clean, and no PR was merged by the builder.

### Departures from the brief (all disclosed, none change the verdict)
- **`gate --help` exits 1.** The brief's step 0 check, "`gate --help` works", technically failed. The builder treated it as passed because running `gate` with no arguments prints the usage line. That's a reasonable call, since the flags exist, but it should have been a stop by the letter of the brief.
- **A drop may cite no evidence.** The brief said evidence has 1–3 items. The plan relaxed this before any call, so only a keep needs a quote.
- **The verifier CLI loads `.env` itself**, so that bin.ts changes by one line. The new bin.ts line is a one-liner, unlike the multi-line branches around it. That's cosmetic.
- **Local main was fast-forwarded** to `9a81505` at step 0.

### Findings worth carrying
- verify-evidence-v1 is lenient: it keeps 61% of the pool, against about 35–40% for verify-ctx-v1.
- **The evidence checks don't tell good findings from bad ones.** Of the 59 findings they dropped, 13 were TP (22%), about the pool's base rate.
- The verifier's own drops were 49 TP of 308 (16%).
- The contracts stage made 0 findings on the 40 dev PRs. Those repos are mostly not TypeScript, so this measures where the stage can reach, not how precise it is.

### Losses
- 065 is a loss: A and B each fail all three conditions. It's the lane's third evaluated precision lever to fail on dev, after 064's vote and 058's verify-ctx.

VERDICT: STOP_FOR_SUNNY

## Checked claims
- 5-run means: B v1 F 38.3 / P 37.2, x1 F 45.7; A v1 F 41.6 / P 40.3, x1 F 50.4; neither passes
  - checked: Read docs/bench/evidence.md; recomputed the means from the per-run rows and checked the count identities
  - result: Confirmed. B F = 191.3/5, A F = 208.2/5, A P = 201.4/5, B x1 F = 228.7/5. Rows sum to comments, and TP+FN = 74.
- Verifier kept 525 of 862, dropped 308, VERIFY_FAILED 29, 140 retried, $2.5640
  - checked: Verifier counts table in docs/bench/evidence.md; per-run sums, and reconciled against gate drops and comment totals
  - result: Confirmed. keep+drop+VF equals the findings in every run. 554 into the gate − 80 drops = 474 = the sum of B's comments, and B-all's 484 and A's 467 reconcile the same way. The cost sum is 2.564.
- Freeze commit 9bfb3eb pushed before the first model call; the prompt and plan are unchanged afterwards
  - checked: git show -s 9bfb3eb 46d12d8; git diff --stat 9bfb3eb d2e5a6b -- packages/analyzer/src/review/ docs/bench/evidence-plan.md
  - result: Confirmed. The commits are dated 08:41:40Z and 08:46:03Z, and the diff after the freeze is empty. I can't check the push time itself.
- Only one line of bin.ts and one line of CLAUDE.md changed; gate/, contracts/ and existing review code are untouched
  - checked: git diff --stat 9a81505 d2e5a6b, plus a diff of bin.ts and CLAUDE.md, plus a diff stat over the protected paths
  - result: Confirmed. The builder's diff is 12 files, bin.ts +1 and CLAUDE.md +1, and the protected-path diff is empty.
- Spend $2.65107 from the ledger delta, under the $5 cap and the sub-caps
  - checked: Relay ledger facts plus the evidence.md spend lines
  - result: Confirmed. $2.5640 + $0.0871 = $2.6511, matching the delta of 23.17092 → 25.82199.
- Gate CLI supports --require-evidence and --min-severity
  - checked: Grep of packages/analyzer/src/gate/cli.ts
  - result: Confirmed at lines 64-71. Also --repo, --base and --max-per-pr.

## Claims not backed by facts
- The freeze push time (08:41:42Z) and the first model call (08:46:09Z) come from the session. I could only see commit times.
- All five dev runs' record counts and the bare repos resolving every head sha and merge base: these are under ~/Projects/tracehound-bench, outside my tools.
- That lanes.json was only read, not edited: I can't read tracehound-ops/ops. Relay's cap of $5 agrees.
- That 828 of 998 judge requests came from the cache, and that 0 requests failed, come from the session.
- That Cal.com was not opened can't be checked from the repo. The docs contain counts only.
- The STOP AND REPORT PR comment on #48 isn't in the relay facts.

## Unrequested changes
- Step 0's '`gate --help` works' failed (exit 1). The builder treated it as passed because running gate with no arguments prints the usage line. It disclosed this.
- The plan relaxed the brief's 'evidence: 1–3 items' so that a drop may cite nothing. Fixed before any model call and disclosed.
- verify-evidence-cli.ts loads .env itself, so that bin.ts changes by one line.
- Local main was fast-forwarded to 9a81505 at step 0.
- The driver's adoption logic and drop-log matching are unit-tested in verify-evidence.test.ts, beyond the tests the brief listed.

## Questions for Sunny
- PR #48 (decision 065, a loss, draft, head d2e5a6b, CI green) is ready. Will you mark it ready and merge it with 'Create a merge commit'? Marking it ready will trigger the dogfood Nebius review, which spends outside the ledger.
- What's next for this lane? The plan has the results doc on 24–25 Oct with a Grok audit. Should 064 and 065 (both dev-only losses) be added to docs/bench/results.md as a docs-only job, or does the lane stay idle until then? $19.18 of the $45 total is left.

## Next prompt (pending: 029/next-prompt.md)
Proposed for Sunny (not sent): after #48 is merged, run a docs-only job, decision 066, in ~/Projects/TraceHound with a fresh session. Add a short 'Precision levers tried on dev (all losses)' section to docs/bench/results.md that cites 058 verify-ctx-v1, 064 vote across runs and 065 evidence verifier + gate, using only their recorded 5-run means and adoption verdicts. Rules:
- $0 Nebius.
- Cal.com is blind.
- May touch docs/bench/results.md, docs/decisions.md (append only) and docs/prompts/066-*.md.
- Open a draft PR, gate on CI, never merge.
relay: tracehound is BLOCKED
  - reviewer verdict is STOP_FOR_SUNNY
  - reviewer has 2 question(s) for Sunny