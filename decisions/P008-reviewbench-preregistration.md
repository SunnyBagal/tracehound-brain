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
