---
id: P008
title: ReviewBench submission — pre-registered config choice and blindness
date: 2026-10-19
kind: experiment
lane: chat
outcome: pending
call: too-early
judged_by:
judged_on:
repo_entry: none (docs/bench/reviewbench.md once rb-01 merges)
pr:
---

# P008 · ReviewBench pre-registration

Written **before** any TraceHound output has been run on any ReviewBench PR (19 Oct 2026, 18:20 IST).

## Blindness
- Nobody on TraceHound (chat, relay, Codex, Claude B, cloud sessions, Grok) opens ReviewBench's `golden/` directory or its judge/classifier prompts, before or after the final run. Tuning on the full set is allowed by ReviewBench's rules; **we don't do it.**
- The only feedback we use before the final run is the portal's **25-PR test run**, a subset of the 219.

## Candidate configs (all use existing, frozen prompts; nothing new)
1. `pool-verify-v1`: what the Action ships.
2. `pool-raw`: the same generators with no verifier.
3. `pool-raw-x2`: pool-raw twice, union, deduplicated.

## Amendment 1 (19 Oct 19:15 IST, before any run): a model dimension
Nemotron 3.5 Super is on Token Factory at the same price ([R10](../research/R10-smarter-reviewer.md)). Two candidates are added, using the same frozen prompts at temperature 0 with only the model id changed:
4. `pool-verify-v1` with `nvidia/Nemotron-3.5-Super-VL` (id to be confirmed in the console);
5. `pool-raw` with `nvidia/Nemotron-3.5-Super-VL`.

That makes five test runs on the 25-PR test set. The selection rule below applies to all five. The cost order for ties is unchanged; same-price models count as equal cost, and the tie goes to 3 Super, the model with the longer record. Selecting among five configs on 25 PRs adds selection bias; we'll state it.

## Selection rule
- Run each config **once** on the 25-PR test run (3 runs).
- Pick the highest **grounded F1**. If two are within 2.0 points, pick the cheaper one (the order is pool-verify-v1, pool-raw, pool-raw-x2).
- Then do **one** final run (219 × 3 rounds) with the chosen config. No second final run unless the first fails for infrastructure reasons, and that would be stated.

## Why recall-heavy configs are in scope
ReviewBench's grounded precision ignores findings that don't match the golden set, so its grounded F1 is driven by recall. Every leaderboard entry has P 84–90%. This is their published methodology, not a loophole. We will publish **grounded and augmented** numbers side by side, and say plainly that the grounded metric favours reviewers that post more.

## Expected (written before any run)
- Grounded recall ≥ 16% would put us above Greptile (27.2) and Cursor (17.3). We guess the 25–30% range (F ≈ 38–44), not promised.
- pool-raw is expected to beat pool-verify-v1 on grounded F1 and lose on augmented precision.

## Budget (Sunny, 19 Oct 18:26: will top up about $25 before the final run, so all three candidates stay)
About $26 of Nebius for the final run plus about $3 for the three test runs. That needs a top-up (Sunny, 19 Oct).

## Amendment 2 (20 Oct 18:45 IST, before any run): candidate 6, a missed-issue pass
Decided by the chat; Sunny delegated the call. Evidence: [R11](../research/R11-grok08.md) (CR-Bench, arXiv 2603.11078: a missed-issue pass raised recall by 5.75 points on GPT-5.2 and 9.2 on GPT-5-mini; closed models only).

6. `pool-raw-missed`: pool-raw, then **one** missed-issue pass (`review-missed-v1`, built in rb-02). The pass sees:
   - the diff chunk;
   - the same repository context review-ctx-v1 builds;
   - the pooled findings.

   It asks only for defects that aren't already listed. Its findings join the pool through decision 058's agree rule. If the pass fails or runs out of time, the output is the pool alone, logged as such.

Rules for candidate 6, on top of the ones above:
- **Frozen first.** The prompt's sha256 is recorded here before the first 25-PR test run of *any* candidate. It is written with no model calls and without reading any ReviewBench data.
- **Model.** It uses the model of whichever of candidates 2 and 5 (pool-raw on 3 Super vs 3.5) scores higher on grounded F1. A tie within 2.0 points goes to 3 Super. So candidate 6 is run last.
- **Stricter margin.** It is chosen only if its grounded F1 beats the best of candidates 1–5 by **≥ 3.0 points**. Otherwise the original rule picks among 1–5.
- **Cutoff.** If rb-02 isn't merged and smoke-tested by **23 Oct 12:00 IST**, candidate 6 is dropped and recorded here as not run. It never delays the test runs of 1–5.
- **Disclosure.** The Action keeps pool-verify-v1. If candidate 6 wins, the results say the ReviewBench config differs from the Action's default, as they would for pool-raw.
- **Cost.** About 1.5× pool-raw. Final run ≈ $33 if chosen. Total ledger stays under the ~$75 cap after the top-up.

Selection bias now covers 6 configs on 25 PRs; we'll state it.

`review-missed-v1` sha256: **`a5a2115f559b82de82c51ee2629e30b05c37ebe0e528e5a02b3de753b42053bc`**. Recorded 20 Oct 19:50 IST, before any test run. Source: PR #51 head `99f35d0`; the chat recomputed it from `REVIEW_MISSED_SYSTEM_PROMPT`, and the prompt has general categories only, no dataset references. Cost estimate revised by rb-02: ≈ $0.027/PR, ≈ $18 for the final run.

## Amendment 3 (20 Oct 20:05 IST, before any run): a fallback if the judge can't be paid for
[R12](../research/R12-reviewbench-portal.md): test runs are judged at our cost (Claude Sonnet 5, roughly $3 per 25-PR run by R7's estimate, so about $18 for six runs).
- If Sunny funds an Anthropic API key, all six candidates run as amended above.
- **If not**, only candidates **2 (pool-raw, 3 Super), 5 (pool-raw, 3.5) and 6 (pool-raw-missed)** run, about $9. The selection rule is unchanged among them, with candidate 6's margin of ≥3.0 points measured against the better of 2 and 5. Candidates 1, 3 and 4 are recorded as not run, with this reason.
- Either way, the per-PR "misses" in test-run reports are **read only to check that runs worked** (status, finding counts). No prompt, threshold or config is changed because of them.

## Amendment 4 (20 Oct 20:50 IST, before any run): if 3.5 can't be called
Job 067 found that `nvidia/Nemotron-3.5-Super-VL` is listed by `GET /v1/models` but every chat completion returns HTTP 404, "does not exist" (35 of 35 calls, $0; FEEDBACK.md on `build/067-dev-3.5`). Therefore:
- If 3.5 is not callable when the test runs start, candidates **4 and 5 are dropped** and recorded as not run (reason: model not callable on Token Factory).
- Candidate 6 then uses **3 Super**.
- If it becomes callable before the test runs start, nothing changes.

## Amendment 5 (23 Oct 17:20 IST, before any test run): every candidate on the partial-results image
Approved by Sunny 23 Oct 17:11 ("yes"). No ReviewBench test run of any candidate has started (portal: "No runs yet"); the only feedback seen is our own smoke runs on PR indexes 0 and 4.

- **Image.** All candidates run on `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:9b9a5127317be79f4bc9f7e771b84b441d887f5cdd52e38e934b8657ceb84508`, built from main `0aeef06` (#57 merge; adapter 70/70 in the build). The configs on `ff22b972` are never run.
- **What changed from `ff22b972`, for the adapter** (`git diff 2a95756 0aeef06` on the files the image runs): only `packages/action/src/pipeline.ts`, `scripts/reviewbench/entry.ts` (decision 068: failed reviewer chunks) and `packages/analyzer/src/llm/client.ts` (#55: 3.5 routed to us-north1). No prompt, threshold, temperature, `max_tokens` or pipeline step changed.
- **The rule, the same for all six candidates.** A reviewer chunk that fails (invalid reply after one retry) is dropped; the PR's output is the findings from the chunks that worked. A pass fails only if **every** chunk of that pass failed (`pool-verify-v1`: both reviewers failed every chunk). This is what the Action ships since #57 and what 067 measured on dev. Before, one failed chunk failed the whole PR.
- **Why now.** 3.5 failed a chunk on 7/40 dev PRs per run (067). Under the old rule candidates 4, 5 (and 6 if it takes 3.5) would lose those PRs outright, under a rule neither the dev measurement nor the product uses. 3 Super rarely fails a chunk (1 failed PR in 4 dev runs), so candidates 1–3 are expected to be almost unchanged.
- **Models per stage.** `RB_CONFIG_MODEL` goes to every stage through `--model`, the verifier included, as in the Action and in 067. Fixes don't exist in the adapter.
- **Disclosure.** Results state that partial reviews are posted, and give the number of PRs with `partial=N/M` where the run logs show it.
- **Earlier amendments.** Amendment 3's fallback is moot: ReviewBench pays the judge on test runs (credentials form asked only for `NEBIUS_API_KEY`), so all six candidates run. Amendment 4 is moot: 3.5 is callable (us-north1, #55). Candidate 6's cutoff (amendment 2) was met: rb-02 merged (#51) and smoke-tested green 21 Oct.
- Selection rule, candidate list and candidate 6's rules are unchanged.

## Amendment 6 (23 Oct 21:55 IST, before any test-run result has been seen): one report-only Nemotron 3 Ultra row
Asked for by Sunny at 21:48. Test run #328 (candidate 2) had started but had no result. **This amendment changes nothing about candidates 1–6, the selection rule or the final run.**

- **Row U (report only, never selectable):** the pipeline of the config the selection rule picks, with `RB_CONFIG_MODEL=nvidia/Nemotron-3-Ultra-550b-a55b`, on the same image `9b9a5127…`. One 25-PR test run.
- **Conditions, all required:**
  - Ultra answers on a Token Factory region the portal's egress allows (us-central1 or us-north1). If it needs routing the image doesn't have, row U is not run, because the image doesn't change for it.
  - It has one green smoke on that image.
  - It runs **after** the final run has finished.
  - The console balance after the final is ≥ $15. Ultra lists at about $1.00/$3.00 per 1M tokens (third-party tracker, unverified), roughly 3× Super. `config/prices.json` has it as a placeholder, so the budget guard uses the $5/$15 fallback.
- **Reporting:** its grounded and augmented numbers go beside the selected config's test-run numbers, labelled "report only, chosen after selection, one 25-PR run". It never changes the final configuration, the Action's model, or any claim about the final run.
- **Not added:** Claude Opus 5.5, GPT/Codex and Kimi K3.
  - Opus and GPT aren't Token Factory models: they need a new egress host (locked at registration, a maintainer change), client code and paid API keys.
  - Kimi K3 lists at about $3/$15 per 1M (third-party), roughly 15× Super's output price, which the balance can't cover alongside the final.
  - For context on frontier closed models, the report cites ReviewBench's existing leaderboard rows instead of running them.

## Amendment 7 (23 Oct 22:45 IST, after one test-run result): the one final run may go to P009's v3
**Seen so far:** only candidate 2's aggregate (test run #328: grounded F1 16.0, P 70.6, R 9.0), far below this plan's expectation (F 38–44). [R16](../research/R16-reviewbench-grounded-f1.md), a research report on what drives grounded recall, followed. It used public leaderboard A/B rows and ReviewBench's methodology, not our test-run detail. A recall-mode configuration (v3) is now being built and pre-registered separately in [P009](P009-reviewbench-v3-recall.md). **It was designed after seeing candidate 2's aggregate score, and the results will say so.**
- Candidates 1–6, their test runs and this plan's selection rule are unchanged. The P008 winner is picked exactly as before.
- **There is still exactly one final run.** It goes to P009's v3 only if P009's rule picks it (v3's test-run grounded F1 ≥ the P008 winner's + 3.0, and v3 meets P009's precision floor). Otherwise it goes to the P008 winner.
- No golden data, judge or classifier prompt is opened; P008's blindness section applies to P009 unchanged.
