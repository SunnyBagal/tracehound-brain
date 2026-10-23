---
updated: 2026-10-23 16:40 IST
updated_by: chat
---

# Now

**Latest handoff: [history/TraceHound_Handoff_Oct23.md](history/TraceHound_Handoff_Oct23.md).** Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

**Schedule change (23 Oct 16:40):** Sunny is in **HackerRank Orchestrate** (24h, 24 Oct 18:00 → 25 Oct 18:00 IST). He's unavailable then.
- **Freeze-2 must finish before 24 Oct 18:00**, or move to 26 Oct.
- The v2 Cal.com run (relay, unattended) can run during the event if it's launched before 18:00 on the 24th.
- **The seeded test moves to 26 Oct** (it needs Sunny).
- 26–27: results + README; 27–28: Devpost + video; submit 29 Oct. No buffer is left.
- **Token Factory console balance: $20.69** (23 Oct), with a top-up of about $50 planned → about $70. Expected need: ReviewBench test runs ~$8, final ~$20–41, Cal.com v2 run ~$4, CI ~$2. That's tight; choose the final candidate with cost in mind.

## 23 Oct 16:40 (Sunny was unwell 22 Oct; no hand steps done that day): #56 merged (main `8d0fb7a`); #57 CLEARED; handoff Oct23
- **#57** (codex-12, decision 068, head `4e3a0a8`): **cleared to merge.** Checked against the source diff:
  - partial results in `runPipeline`; it fails only when both reviewers fail every chunk;
  - the footer field `chunkFailed {diff, ctx}`;
  - `...pipeline` replaced by explicit `pool`/`kept`, so the key order and bytes are unchanged when nothing fails;
  - `review-diff-v1` keeps all-or-nothing;
  - the `model` default is 3.5; `FIX_MODEL` is unchanged;
  - the adapter's pool-raw / x2 / missed keep successful chunks, and a pass fails only if all its chunks failed;
  - the smoke regex is `^ok( partial=N/M)?$`.

  Action 237/237, adapter 70/70, adapter tsc clean (Node 22). [Report](prompts/2026-10-22/codex-12-068-default-3.5.out.md).
- **Sunny:** top up about **$50** on the Nebius console (decided). P008 amendment 5 is still unanswered. Next: merge #57 → rebuild image → 3.5 smoke → re-add portal configs on the new digest → freeze-2 (24 Oct, tag `review-freeze-2`, re-pin demo and dogfood).

## 22 Oct 04:15: 067 says ADOPT 3.5, but the Action doesn't match what was measured
- **#56** (relay 067b, head `46a678d`, CI green): **cleared to merge.** Chat recomputed every P/R/F from the TP/FP/FN columns (7r v1 50.72, 8 v1 50.00, mean 50.4; x1 54.41/55.88 → 55.1). The rule passes on 2 runs (run 9 was cut by the cap, so the fallback applies). Bootstrap v1 +8.1 [+1.4, +14.4]; x1 +3.9 [−4.2, +11.1] includes 0. Both 3.5 runs beat every one of the five 3 Super runs. The gain is precision (half the FPs). Cost per PR about 2× (median $0.063). Spend $7.98; **ledger $33.81 / $45**.
- **Catch (chat):** the bench scores a PR with a `CHUNK_FAILED` chunk **with partial results** (`review.ts:5`), and 3.5 failed a chunk on 7/40 PRs per run. The **Action** (`pipeline.ts`: throw on `chunkFailed > 0`) and the **ReviewBench adapter** (all-or-nothing) post **nothing** in that case. Switching the default alone would ship an unmeasured config that is silent on about 1 in 6 PRs. → [codex-12 / decision 068](prompts/2026-10-22/codex-12-068-default-3.5.md):
  - partial results in the Action and the adapter;
  - `model` input default → 3.5;
  - fixes stay on Super;
  - byte-identical when nothing fails.

  It must land before freeze-2 (24 Oct).
- **P008 amendment 5 (proposed, Sunny to approve, before any test run):** candidates run on the image with 068's partial rule (all candidates alike). Without it, 3.5 candidates 4/5 lose about 17% of PRs outright on ReviewBench, under a rule that 067 didn't use.
- **Relay's 3 questions, answered:**
  1. Merge #56: yes.
  2. The default switch goes to Codex (owns the Action), not relay's proposed 068.
  3. Relay Nebius cap goes to **$0** until the v2 Cal.com run brief (about 25 Oct), which gets its own cap.
- **Money:** 3.5 doubles cost. ReviewBench final on 3.5 pool-verify ≈ 657 × $0.063 ≈ **$41** (was $21). That's on the separate `reviewbench` key, so the **console balance needs a bigger top-up** before the final.

## 21 Oct 21:32: #55 merged → main `da13585`; image `bc6530f2`
- **ReviewBench image with 3.5 routing:** `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:bc6530f2d2fc9379a55f466be9917b8e9bf51dd5816fa90db0c8f759bbe6d0eb`, built from `da13585` (#55 merge). Adapter 47/47, public. Use it for smoke run 4 (3.5) and candidates 3–6.
- Smoke on `bc6530f2` (21:42) ran **3 Super by mistake** (model input left at default): pool-raw green, idx 0 2 findings $0.0422 175 s, idx 4 1 finding $0.0044 27 s. That confirms #55 leaves 3 Super unchanged. **Smoke 3.5 (21:54), pool-raw on `bc6530f2`: RED.** The routing works: the calls reached 3.5. idx 4 ok, 1 finding, $0.008, 12 s. idx 0 `failed: review: failed chunks (first: the reply is not valid JSON; after one retry: the reply is not valid JSON)`, 0 findings, $0.0957, 159 s. Likely reasoning using up `max_tokens` 32k (unverified, no `finish_reason` kept). That's model behaviour, so it doesn't trigger P008 amendment 4 (3.5 is callable). → [067b](prompts/2026-10-21/067b-resume-dev-3.5.md): resume 067 on dev with a pre-registered stop-rule amendment (failed chunks count as the result; stop if more than 10/40 PRs fail or on any request error). Verdict by 23 Oct.

## 21 Oct 21:00: #55 (bunny-03) CLEARED, CI green (21:07): merge it
- **#55** `fix/model-base-url` head `5ec764d`, 4 commits. Checked by chat:
  - `baseUrlFor` rewrites only Token Factory hosts (`^api\.tokenfactory\.[a-z0-9-]+\.nebius\.com$`) and only exact table ids (`Object.hasOwn`).
  - Probes: a lowercase id, 3 Super, a look-alike attacker host, localhost, `__proto__`/`toString`, and a non-URL all stay unchanged.
  - The cache key is sha256(model + request), with no URL, so it's unchanged.
  - Analyzer guardrail tests 25/25, adapter 47/47, adapter tsc clean (Node 22).
  - The FEEDBACK entry sticks to facts.
  
  **Merge when CI is green**, then rebuild the ReviewBench image → smoke run 4 (pool-raw on 3.5) → the 067 resume prompt. Bunny's report is filed ([out](prompts/2026-10-21/bunny-03-model-base-url.out.md)): workspace tests action 226 / analyzer 869+7 / viewer 188, adapter 47; a 3 Super cache-key pin test was added.

## 21 Oct 20:35: bunny-03 written
- Image rebuilt on `7de5dbd` (20:52): `sha256:8e1ec5f267a02e8448a32309e1a054f128b93b2d0ce1efe0ea80ae7ae85b166d`. It was built **before** bunny-03, so it has no 3.5 routing. ReviewBench behaviour is the same as `ff22b972` (#53 touches only the Action). Don't use it for 3.5, and don't register it.
- [bunny-03](prompts/2026-10-21/bunny-03-model-base-url.md): per-model base URL in the client (3.5 → us-north1), adapter log, docs, FEEDBACK entry. Draft PR. After merge, Sunny rebuilds the ReviewBench image → smoke run 4 (pool-raw on 3.5) → the chat writes the 067 resume prompt (relay). 067 must run by 23 Oct.

## 21 Oct 17:10: #53 CLEARED → **merged, main `7de5dbd`** (verified 20:30)
- **#53 head `3b196fa`: merge it** (mark ready, merge commit). codex-11 ([report](prompts/2026-10-21/codex-11-int-01-fixes.out.md)): 7 commits, one per Ultrareview finding. The chat read the source diff against ur-02:
  - SIGTERM, then SIGKILL after 5 s (Node's own timeout is off; a timer of its own sends the kills). `shallow.lock` is removed only after our own SIGKILL, and only if it didn't exist before.
  - The fetch needs at least 30 s left. The analyzer gets the remaining time minus 10 s, and is skipped below 5 s.
  - A typed check skipped for budget or error keeps the keyed findings, with a `typed` footer field.
  - The section comes after the disclaimer; with `contracts` undefined the order is unchanged.
  - `escapeContractText` covers `& < > [ ] \` and backticks, and breaks `@`.
  - No checkout (or `rev-parse --git-dir` failing) gives status `off` and no delivery, so output is byte-identical.
  - Docs say "always body findings".
  - Action tests **226/226 locally (Node 22)**. The demo branch is pinned to `3b196fa` (`303b689`), trigger commit `71c5f49`. Section-after-disclaimer is checked in code and tests; WebFetch only rendered the old 20 Oct review.
- **Residual, backlog (analyzer lane):** `--budget-s` bounds only the **typed** check (`computeContracts`), not keyed or dependents. If those run long, the outer SIGTERM still lands. The Node analyzer has no SIGTERM handler, so its `finally` (closing the worktrees) doesn't run, and worktrees can still leak in that case. That's rare and cosmetic; the fix needs a change in `packages/analyzer`.
- **Next for freeze-2 (24 Oct, tag `review-freeze-2`):** re-pin the demo `main` and dogfood workflows to the merge commit **with `model: nvidia/nemotron-3-super-120b-a12b`**. Demo PR #6's review still lists Nano generators.

## 21 Oct 00:10: HOLD #53 → codex-11; smoke runs on 3 Super green
- **HOLD #53.** Ultrareview #2 ([output](prompts/2026-10-20/ur-02-int-01.out.md)) found 7 issues. The chat checked all 7 against `747a3a7` and confirmed them all. Fix round [codex-11](prompts/2026-10-21/codex-11-int-01-fixes.md) runs on the same branch. Findings:
  1. SIGKILL can leave `.git/shallow.lock`, which breaks a data-branch step in the same job.
  2. `--budget-s 90` doesn't subtract the time already spent, so worktrees leak.
  3. A typed check skipped for budget (more than 5,000 root files) drops the keyed findings.
  4. The contracts section sits before the disclaimer and gets swallowed by `learn` `bodyFindings`.
  5. Unescaped reason text: `<User>` is stripped and `@team` pings that team.
  6. With no checkout, a review-diff-v1 review now gains a footer.
  7. The inline path is unreachable, because the analyzer reports only unedited files. That's a docs fix.
- **Not covered by anyone yet:** a hostile tsconfig (`extends`, `plugins`) or symlinks in the PR. The focus text never reached the Ultrareview. Risk is low: fork PRs are skipped, so the PR author already has write access. Backlog.
- **Ultrareviews: 1 left** (reserve).
- **ReviewBench portal (21 Oct 00:07): registered**, identifier `tracehound`, awaiting approval (a review-request PR on their repo). Config 1 = `pipeline=pool-raw`, `model=nvidia/nemotron-3-super-120b-a12b`, digest `ff22b972…`. Credentials are per configuration. The portal page says **"ReviewBench provides the evaluation judge"**, so test runs may not need our Anthropic key, unlike R12. Confirm from the credentials form (only `NEBIUS_API_KEY` expected). If so, amendment 3's fallback is moot. **Credentials form asked only for `NEBIUS_API_KEY`** (Sunny used a separate `reviewbench` Nebius key, stored for config 1). So ReviewBench pays the judge on test runs too: **no Anthropic key needed, and P008 amendment 3's fallback is moot** (all candidates can test-run). R12's "self-funded" was wrong. Configs (00:28): **candidate 2** `pool-raw` and **candidate 1** `pool-verify-v1`, both 3 Super on `ff22b972`, labels exactly `pipeline` + `model`, credentials stored. Candidate 3 (`pool-raw-x2`) can be added the same way; 4–6 wait for the 3.5 fix. Onboarding PR **review-bench/ReviewBench#92** (open, checks green, awaiting code-owner review). Manifest verified from Sunny's screenshot: full digest `…e1276b23ba` (public), 1 env secret, Model API us-central1, **egress us-north1 + us-central1**, 1800 s.
- **Smoke runs on `ff22b972`, all on 3 Super, all green:**

  | pipeline | idx 0 (TS) | idx 4 (Py) |
  |---|---|---|
  | pool-raw | 2 findings, $0.0586, 284 s | 1, $0.0040, 16 s |
  | pool-verify-v1 | 0, $0.0436, 208 s | 1, $0.0047, 39 s |
  | pool-raw-missed | 1 (pool 0, missed 1, new 1), $0.1021, 607 s | 2 (pool 1, missed 1, new 1), $0.0066, 28 s |

  About $0.22 of CI spend, outside the local ledger. idx 0's pool varies between runs (2 vs 0 at temperature 0), the known spread. pool-raw-missed took 10 min on idx 0, against the 25-min limit. The 3.5 smoke run waits for the per-model base URL fix.

## 20 Oct 23:00: image rebuilt on main; Codex extended
- **ReviewBench image on main `2a95756`** (includes #51): `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:ff22b972feb243d7c4f73db69ced12ee4a811e66f552a094e77794e1276b23ba`. Public; adapter tests 46/46. Warnings are harmless (Node 20 deprecation, the `FROM --platform` lint). Use it for smoke runs 1–3 and the portal registration. The per-model base-URL fix will produce a newer digest, which is added in the portal with no re-approval.
- **Smoke runs on `ff22b972`:** #1 failed at the image check (a stray space in the input; $0). **#2 pool-raw / 3 Super: green.** idx 0 (TS): ok, 2 findings, $0.0586, 284 s; idx 4 (Py): ok, 1 finding, $0.0040, 16 s. pool-verify-v1 and pool-raw-missed are next.
- **Codex lane extended:** Sunny has ChatGPT Plus for 1 month (free). Codex no longer ends 22 Oct.

## 20 Oct 22:40: 3.5 found. It is on us-north1
- **Nemotron 3.5 Super VL answers on `https://api.tokenfactory.us-north1.nebius.com/v1`** with id `nvidia/Nemotron-3.5-Super-VL` (Sunny's curl, 22:37: 200, reasoning on). **3 Super is not on us-north1** (`model_not_found`), and 3.5 is not callable on us-central1, even though us-central1's `/v1/models` lists it (Nebius bug, FEEDBACK).
- **Effect:** the client has one global base URL (`DEFAULT_BASE_URL` us-central1, or `NEBIUS_BASE_URL`), so no run can mix the two. 067 needs 3.5 reviewers plus Super/Qwen judges. ReviewBench candidates 4/5 need 3.5, and 1–3 need 3 Super.
- **Fix (proposed):** a per-model base URL in the client (3.5 → us-north1, else the default), used by every stage and the ReviewBench adapter; the smoke workflow passes it through. No network in tests. Then 067 resumes and smoke run 4 runs.
- **Portal:** register with Model API URL **us-central1** plus other egress host **`api.tokenfactory.us-north1.nebius.com`**. Declare it now, because the docs don't say egress hosts can be edited later.

## 20 Oct 21:40: #53 and #54 checked; #52 merged
- **main `2a95756`** (#52 merged, verified). #42 closed unmerged by Sunny (21:33).
- **Ultrareview #2 on #53: run it** ([ur-02](prompts/2026-10-20/ur-02-int-01.md), new cloud session). Then **1 Ultrareview left**, kept in reserve for its fix round or an emergency. No later PR before submit is bigger: #53 is the last product code that runs on untrusted PR code by default.
- **#53 int-01 (Codex), head `747a3a7`: chat check passed, no HOLD.** Merge after Ultrareview #2 (cloud). Run it **tonight**: Codex ends 22 Oct, so a fix round has to start tomorrow.
  - Verified: 13 files, all in lane; Action tests 214/214 on Node 22. `contracts: 'off'` is byte-identical: hard-coded legacy payloads for both reviewers and both fixes modes, and no off-path change in the diff. It runs concurrently and uses the merge base. It is fail-soft with one log line and doesn't apply `learn`. Inline is capped at 10, outside `max-comments`. The ±3 dedupe keeps the contract finding. Credentials are stripped (tokens, askpass, `extraheader` reset for the fetch).
  - Live: demo PR #6 is pinned to `747a3a7`. Its review body lists 4 dependents (`index.ts:13`, `scripts/repair-blocked-metadata.ts:22`, `test/linkDetector.test.ts:4`, `worker.ts:10`, all TS2305). The 2.255 s is in the hidden footer and wasn't checked here. Demo PR #6 must never merge.
  - Backlog (not blocking):
    1. The dedupe matches against every contract finding, including ones shown only as "and N more", so a model finding can vanish while its contract is hidden. This needs more than 10 body findings.
    2. The body's reason text isn't Markdown-escaped (the path label is). Route, env and event keys come from PR code, so a PR can inject links or @mentions into the bot's review.
    3. A typed check skipped for budget or error marks the whole stage failed and drops the keyed findings too, so big repos always report failed.
    4. The review checkout is depth 1, so every run takes the credential-free unshallow fetch. Private repos fail soft (documented: `fetch-depth: 0`).
    5. The footer has no `status: "off"` when off. That's needed for byte-identical output, and it's documented. Accepted.
- **#54 bunny-02 judges' README, head `db83286`: fine, but leave it a draft.** Merging now would put `[[RESULTS-TABLE]]` placeholders on the public README. The fill job (26 Oct) must:
  - change the try-it pin from `@main` to the freeze-2 tag;
  - drop "landing in int-01 (draft PR #53)" once #53 merges;
  - drop the 3.5 line if 067 is dropped;
  - remove the second `# TraceHound` H1 in Details;
  - check the Mermaid render.
  - Wording passes: no "beats", no "3rd of 6".
- **Demo and dogfood don't run the benchmarked model.** Neither workflow sets `model`: the demo pins `41a92a8` on main, and PR #6 and dogfood pin `e4e3e3a`. So both generators run on **Nano** (`DEFAULT_MODEL`), and only the verifier and fixes use Super. Demo #6's review header lists both models. The benchmark is Super. Bunny caught this in the README.
  - **Fix at freeze-2:** re-pin the demo and dogfood workflows with `model: nvidia/nemotron-3-super-120b-a12b`. No code change, no freeze risk.
  - **The seeded test (25 Oct) must set it too**, or it measures Nano generators.
  - The alternative, making Super the Action default, is a code change for Codex by 22 Oct. Sunny decides; chat recommends the workflow fix.
- **Portal: the Model API URL is locked after registration** (only a maintainer can change it, via the onboarding PR; ReviewBench ONBOARDING.md step 5). Sunny confirms 3.5's base URL in the playground code sample **before** registering. If it isn't `https://api.tokenfactory.us-central1.nebius.com/v1`, the chat decides the URL first. If 3.5's model id differs from `nvidia/Nemotron-3.5-Super-VL`, the smoke workflow's `model` choice list needs a one-line PR.
- **Tag name:** `eval-freeze-2` already exists (4 Oct, repair eval). The 24 Oct tag is **`review-freeze-2`**.

## Repo state

- **TraceHound main: **`b98f753`** (verified 19 Oct 13:55, #45 merge). Earlier: `5857179` (#47), `a9d16b5` (#46), `2af8c3a` (#43).** Before that `c6c12b9` (#44 merge). Older: main `e7309a5` (verified 18 Oct 00:10): #33 merge `6b4276e`, #34 merge `e7309a5`. Tag `review-freeze-1` → `6b4276e` (#33's merge commit), tagged 17 Oct 22:14 IST. #35 merged 18 Oct (`3113c3a`). #36 → `d37ffa8`, #32 → `e95ef20`, #37 → `df4f915` (all merged 18 Oct). #38 → `41a92a8` and #39 → `6058d53` (both merged by Sunny 18 Oct ~20:25, merge commits; #38 before codex-06c, by accident; harmless: suggestions work, #2's just isn't committable). #40 merged (`e4e3e3a`).
  Open:
  - **#43** codex-07 data branch, head **`19ece32`** (12 commits; ur-01 fixes on top of `d733a6a`). **Re-checked by chat 19 Oct 00:10:** source diff read against ur-01; #1 merge-base diff, #2 monotonic latest + state rank, #3 ref-lock retry, #4/#8 split `git`/`branchGit` from `cleanGitEnv()`, #9/#10 last-line footer + JSON temp file all match; #5 backlog line; #6/#7 example split into review (cancel-in-progress) and data (serialized, only `contents: write`) jobs. Action tests 153/153 and typecheck re-run locally; merges cleanly with main `c6c12b9`. `max-comments: '0'` returns before any GitHub/Nebius call (verified in `action.ts`). Accepted departures: reopened → open; #5 own commit; extra re-run row. **Merged 19 Oct** (`2af8c3a`). Residuals, backlog only: a manual re-run of an old `reopened` after a later close flips the entry back to open; a late event for a new head after close adds an open entry; dropped pending data jobs can leave a closed PR `open` (documented); the split example records no `reviewFooter` (viewer doesn't read it today; ui-03 must not depend on it). Dogfood pin still `e4e3e3a`.
  - **#44** ui-02 dashboard: **merged** (main `c6c12b9`, verified 19 Oct 00:05). Re-sync `viewer/lib/benchmark.ts` after the R6 wording fixes land (it still shows "3rd").
  - #41 (cc-01 audit) and #42 (ReviewBench spike) are drafts; their content feeds later jobs. Next relay: **064 voting across runs** (`064-vote.md`, cap $10, dev only, PR as draft). Next Codex: `codex-06c-deleted-lines.md` (rewritten against main, new branch `codex/fix-committable`).
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $39.93** (18 Oct 21:00; was $46.22 on 17 Oct; −$6.29 = 062's $5.31 + about $0.98 of off-ledger CI: dogfood + demo live checks); ledger **$23.17092** after 064 (19 Oct; was $12.45236). Caps: total stays at the $45 default (ledger + $15 fits; decided 17 Oct), per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

## In flight

#33 and #34 merged with merge commits: **verified on GitHub 18 Oct**.

- **#35** `ui/viewer-round-1` head `acc5752`, 18 commits: checked by chat 18 Oct. Lane paths only; $0 Nebius; no deps; snapshots at Recall `20ac149` and demo `efc5844` = their mains; CI green (the prisma annotation comes from the analyzer fixture and was already there). One red intermediate commit (`f6df5a9`), disclosed. Nits go to ui-02: edge labels collide, titles truncate, "1 modules". Step 9 now goes to a **new branch** via `ui-01b-after-062.md`.
- **062 done → PR #36** (head `9c0624f`, CI green). Checked by chat 18 Oct 03:40:
  - all 16 table rows recompute, and the spreads are consistent;
  - frozen paths are unchanged against `review-freeze-1`;
  - the predictions match 061's wording;
  - no Cal.com text.
  - Relay's BLOCKED was only "main moved" (#35), which is harmless. `tracehound/025` is relay's own job counter (brief 062).
  - **Headline: blind Cal.com run 1, v1: pool-verify-v1 F 58.3% (P 56.0 / R 60.9), 3rd of 6** (augment 73.2, cursor 66.7 | greptile 50.0, copilot 46.2, coderabbit 36.7). It was 3rd in 5 of 6 run×judge cells and 5th in x1 run 3 (47.3).
  - Predictions: (a) right, (b) wrong, (c) right.
  - Dev spread: 35.6 → 48.0 across two runs (larger than the 0.6-point adoption margin).
  - Dev run 2 was void (relay's 2 h background limit killed it), then 2r; dev run 3 was stopped at the cap.
  - Spend $5.31; **ledger $17.76**.
- 063 → PR #39 (relay 026). Checked by chat 18 Oct: 4 small commits; ranks, spreads and costs trace to the repo.
  - Wording nit for the judges' README job: section 1 says x1 "changes nothing". It means it decides nothing; x1 reverses (a) and gives a 5th in run 3, both stated in §3/5/6.
  - Relay lane is idle until the judges' README (21–22 Oct).
- **Dogfood spend is off the local ledger** (`secrets.NEBIUS_API_KEY` in CI), roughly $0.03–0.25 per PR. Decided by chat: keep it on (it's live demo evidence) and track it via the Nebius console. Any future job that needs "no other Nebius use" opens its PRs as drafts (the workflow skips drafts).

| Lane | Job | Status |
|---|---|---|
| Relay | **062** Cal.com run (`briefs/062-test-run.md`, cap $15): fetches → tag `review-freeze-1` on #33's merge commit → Cal.com run 1 (headline) → runs 2–3 → dev runs 2–3 → results doc | Starting 17 Oct night. Needs `nebiusCapUsd` = 15, #32 unmerged, no other Nebius use. Takes hours |
| UI lane | **ui-01-codex.md**: round 1 + addendum merged, now **Codex GPT Sol 6.1** in ~/Projects/TraceHound-ui, branch `ui/viewer-round-1` | Starting 17 Oct night. No Nebius until Sunny writes "062 is done"; step 9 (Super summaries, $0.20) comes after |
| Codex | #32 dogfood workflow + demo doc | **Hold until 062's PR is done** |
| Codex queue | codex-06 fix suggestions → codex-07 data branch (persistence) | after 062 (its Action runs use Nebius) |
| Grok | grok-04 X post draft | Post Mon 19 Oct; Cal.com numbers only once 062's results doc has merged and the chat has checked it |

## Known problems

- **Codex automatic review** has posted nothing on #27–#31. Check the Review trigger, or comment `@codex review`.
- **Demo (17 Oct, pin `50e1d4a`):** the reviewer commented on the seeded line in 2 of 4 demo PRs (#2, #3), missed #1 and #4, and posted 0 comments on the control (#5). Dogfood #32: 1 comment.
- **No live reviewer demo yet**, and judges may score from the video and text alone (R3).
- **X post planned for Monday 19 Oct** (benchmark thread; tags to verify).

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 done (adopted) | #28 merged |
| 17–18 Oct | 060 scorer fairness: augmented precision plus a second judge | Demo repo with the action installed and real review comments (the judges' working demo); switch the action to the reviewer that wins |
| 20 Oct | Results doc (moved earlier) | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the one Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md, "what changed since 26 August" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct (deadline 30 Oct 22:30 IST) | |

## Open decisions

- [P004](decisions/P004-target-reset.md): **rejected 18 Oct.** "Beats" is allowed, scoped to the test, run and scorer. The goal is the top 2 blind (Cursor 66.7 / Augment 73.2 on Cal.com). **18 Oct chat update:** the blind result (3rd of 6, ahead of Greptile in 5 of 6 cells) makes the honest framing *stronger*, not weaker; still no "beats" headline because of the spread.
- Decided 2026-10-16 (Sunny): (a) first, as 059 pool-verify-v1. Next after it: 060, augmented precision for every reviewer plus a second judge.
- Decided 2026-10-16 (Sunny): the Action switches to pool-verify-v1; the 060 second judge is Qwen3-235B-A22B-Instruct-2507; 060 covers the second judge plus augmented precision.
- Decided 2026-10-16 (Sunny): **stop tuning, freeze now**; demo on **both** TraceHound itself (dogfood) and a seeded-bug demo repo.
- Cal.com run: pool-verify-v1 primary. Chat recommends declaring diff-v1 and pool-raw as secondary rows in advance; they come free from the same pipeline.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30, and decisions 001–031 first appear that same day, so they were likely carried over from earlier work. Is the earlier history (FixFlow) somewhere public? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.

## v2 architecture (19 Oct, proposed, Sunny to approve)

See [project/architecture-v2.md](project/architecture-v2.md): S1 pre-filter → S2 contract map → S3 generators → ~~S4 vote~~ (lost in 064) → S5 evidence verifier → S6 post-gate → S7 deliver → S8 learn. Freeze-2 is on 24 Oct, followed by **one** blind Cal.com run of v2. **No standalone Cal.com run for 064** (nothing was adopted). Ultrareview #2 moves to the int-01 branch. Codex and UI end 22 Oct; Claude B takes integration and UI.

## Open PRs (checked by chat 19 Oct 03:40; all three merge cleanly with main and with each other; merged together locally, analyzer 724 tests pass and typecheck is clean)

- **#46** 064 vote, head `d76eeb3`, draft, CI green. **Loss, recorded as one. Merge it** (mark it ready, which runs the dogfood review for a few cents; use a merge commit).
  - Every table recomputes: comments = TP+FP+dup+low and TP+FN = 74 in all 18 rows.
  - The means recompute: v1 F 42.3 / P 40.5; x1 F 51.3 / P 50.9.
  - Commit order: plan `57f0b91` at 21:09 IST, then `vote.ts` at 21:13 IST, both before any run finished.
  - Accepted: two test files under `packages/analyzer/test/`.
  - **Not accepted:** the builder edited `tracehound-ops/ops/lanes.json` (cap 0 → 10). 065's brief now forbids any edit to tracehound-ops. Sunny: check what lanes.json says.
- **#45** codex-08 gate (+08b): **merged by Sunny 19 Oct 13:46 → main `b98f753`**. Chat re-ran the full suite on main afterwards: action 153, analyzer 778 (+7 skipped), viewer 186; typecheck clean; the `gate` usage line has `--base`, and `contracts` prints its usage. Main CI run 39221589986 was in progress at 13:55.
- **#41** cc-01 audit: **merged** (in main `9a81505`).
- **#48** 065 evidence verifier + gate, head `d2e5a6b`, draft, CI green. **A loss, recorded as one. Merge it** (mark it ready, merge commit). Checked by chat 19 Oct 16:20: all 40 table rows recompute; means v1 baseline F 42.3 / P 40.5, A 41.6 / 40.3, B 38.3 / 37.2; x1 baseline 51.3, A 50.4, B 45.7. Freeze `9bfb3eb` came before the driver and the first call, and the diff after it is empty. Spend $2.651 → **ledger $25.82**. The evidence checks dropped TPs at the pool's base rate (13 of 59), and contracts made 0 findings on dev (mostly non-TS repos).
- **#42** ReviewBench spike (draft, `scripts/reviewbench/` + doc): **leave as a draft** until Sunny decides on ReviewBench (it needs Anthropic API credit: ~$3 for the practice set, ~$7 for a 60-PR run). If it's a no by 22 Oct, close it unmerged; R7 keeps the findings.
- **#47** cc-03 contracts, head `d469e28`, draft. Paths are in scope; 141 tests; 10 breaks caught, 0 findings on the 10 controls; 0 findings on 9 real commits; excalidraw takes 53–65 s against a 120 s budget. **Merge** (mark it ready). Known gaps are written up: package types are `any` without node_modules, and route mount prefixes are ignored.

**Blindness (from 19 Oct):** no TraceHound lane (relay, Codex, Claude B, cloud builders, Grok) and not the chat may open `linea-seeded`, `recall-seeded` or `tracehound-seeds` until scoring starts on 25 Oct. Every brief from now on says so.

## Credit (20 Oct 18:50)
**Claude cloud: $52** (Sunny). The handoff's ~$85 was two days stale; most of the drop is the rb-01 session (#50), not this chat. A big builder job costs roughly $20–30 of cloud credit. From now on: rb-02 and int-01 go to **relay** (Pro plan), and the cloud is only for rb-01b. The chat keeps PR checks lean.

## HOLD #49 (20 Oct 19:05): codex-09b first
Checked by chat on `30a3d44`. Paths are in scope; analyzer learn tests 47/47 and action tests 174/174 pass; `learn: 'off'` is unchanged; the permission check is right.

**Defect** (it came from the brief): code spans are dropped, so identifiers vanish. The probes:
- a rejected null-deref on `user` suppressed a null-deref on `order` in the same file;
- it also suppressed a template-alike comment in another `.ts` file.

Fix: [codex-09b](prompts/2026-10-20/codex-09b-learn-fixes.md), in the same Codex task. **Must land by 22 Oct.**

Grok bots: [grok-09](prompts/2026-10-20/grok-09-bots.md) has three jobs:
- A: ReviewBench portal questions. Includes who pays the judge, and whether we can re-register a digest for candidate 6.
- B: X and LinkedIn drafts, with the Verifier checking numbers against results.md.
- C: a daily digest.

## 20 Oct 20:50: 3.5 not callable; relay stuck; #52 nearly done
- **Nemotron 3.5 Super VL: listed but HTTP 404 on chat** (067 builder: 35/35 calls failed, $0). `GET /v1/models` lists it next to 3 Super, which works. 067 stopped correctly after run 7's first calls (branch `build/067-dev-3.5`, `cf5b70b`). **Sunny:** try it in the Token Factory playground, copy its exact API id and base URL from the model page's code sample, and ask Nebius support with the FEEDBACK entry. P008 amendment 4: 3.5 candidates are dropped if it's still uncallable when the test runs start. Smoke run 3 (3.5) will go red until it's fixed.
- **Relay is stuck by design, not a bug:** it polls CI for `cf5b70b`, but CI runs only on `main` pushes and PRs, and the branch has no PR. **Ctrl-C the relay.** 067 resumes once 3.5 answers.
- **#52 (Bunny 066): cleared to merge** at `e91a268` (066c fixed the three leftovers; tests green locally). Nit: `BENCHMARK_REVISION` pins `e54ee63`, whose tables are identical; re-pin it in any later viewer job.
- **Devpost ([R14](research/R14-devpost-checklist.md)):** **no competitor names or logos in the video** (trademark rule); the demo must work without a judge's key; MIT LICENSE exists.
- **Terms ([R13](research/R13-tool-terms.md)):** Cursor allows publishing benchmarks with a full replication kit; Greptile §1.4(a) bars sharing "any results" without OK (ask Greptile); CodeRabbit is Sunny's call (kept).

## Lanes and jobs (20 Oct 20:20)
| Lane | Job | Status |
|---|---|---|
| **Sunny** (you, by hand) | close #42; rebuild the ReviewBench image on main → 4 smoke runs; register in the portal; decide on the judge API key (~$20) or the P008 amendment 3 fallback; send Verifier B | now |
| **Bunny** | [bunny-066b](prompts/2026-10-20/bunny-066b.md): finish 066 (viewer sync, CodeRabbit wording fix) | same session |
| **Codex** | [codex-10 int-01](prompts/2026-10-20/codex-10-int-01-contracts.md): contracts in the Action, default on, fail-soft. Gate and evidence verifier **not** wired (065 lost) | new task, by 22 Oct |
| **Claude cloud** | Ultrareview #2 on int-01's PR once it opens. Otherwise reserve | waits |
| **Grok bots** | [grok-10](prompts/2026-10-20/grok-10-bots.md): D (benchmark clauses in Greptile/Bugbot/CodeRabbit terms), E (Devpost submission checklist) | now |
| **Sunny's Claude Code (relay)** | [067](prompts/2026-10-20/067-dev-nemotron-3.5.md): Nemotron 3.5 on dev, 3 runs, cap $8, pre-registered adoption rule (≥ +3.0 F, P not lower, x1 not lower). Moved earlier: it's dev-only, so it can't bias ReviewBench | **now** |
| **Bunny, next** | [bunny-02](prompts/2026-10-20/bunny-02-judges-readme.md): judges' README with number placeholders | after 066b's PR |

**Post plan (Sunny, 20 Oct):** one LinkedIn and X post once the numbers are in, about 26 Oct: ReviewBench final, the 3.5 dev verdict, the seeded test and the v2 blind Cal.com run. Sunny will probably fund the Anthropic API key for the test-run judge.

**Chat decision (20 Oct):** v2 in the Action = pool-verify-v1 + **contracts** (+ `learn`, off by default). The gate and the evidence verifier stay out (065). This is what freeze-2 (24 Oct) and the seeded test (25 Oct) run.

## 20 Oct 19:50: #49 and #51 cleared, #50 merged
- **#50, #49 and #51 merged** (main includes `review-missed` and learn `dee2bed`). #50 → `8193651`. Next: rebuild the image on main, then 3 smoke runs (pool-raw 3 Super, pool-verify-v1 3 Super, pool-raw 3.5) **plus a 4th, pool-raw-missed on 3 Super, after #51 merges**.
- **#49 cleared:** head `9b21e70`, chat re-probed. A different variable in the same file and template-alike comments in other files are now **kept**. Learn tests 60, action 179. Merge it (mark ready), then int-01.
- **#51 cleared** (rb-02, candidate 6): head `99f35d0`. The chat recomputed the prompt sha256 (`a5a2115f…53bc`, now in P008). Frozen prompt files and `packages/action` are unchanged; missed tests 10, adapter 46. The prompt is generic. rb-02 ran in the cloud, not relay. Merge it.
- **Judge cost ([R12](research/R12-reviewbench-portal.md)):** test runs are judged at **our** cost (Sonnet 5, about $3 each, so about $18 for 6). The final run is paid by ReviewBench. **Sunny decides:** fund an Anthropic API key (all 6 candidates), or take the fallback in P008 amendment 3 (candidates 2, 5 and 6 only, about $9). Register in the portal **now**; approval takes up to a business day and a new digest later needs no re-approval.
- **Test runs may start** once the 4 smoke runs are green and the new digest is registered.
- **Builders report via the brain:** Bunny (and Sunny's Claude Code) write their final report to `inbox/` only and push it. The chat files it, checks the PR, and updates NOW/prs. Builders never edit anything else ([inbox/README](inbox/README.md)).
- **#42** (ReviewBench spike, draft): close it unmerged now that #50 has merged; R7 keeps the findings.
- Names: **Sunny** = account A's local Claude Code; **Bunny** = account B (`claudeb`).

## Lanes (20 Oct 19:30)
- **Relay:** rb-02, started 19:22 by Sunny. Its step 0 needs #50 merged.
- **Claude B:** a second Pro account, set up per [claude-b-setup](prompts/2026-10-20/claude-b-setup.md), in `~/Projects/TraceHound-claude2` with `CLAUDE_CONFIG_DIR=~/.claude-b` and no `.env`. First job: **066** (moved from relay, Sonnet). Then int-01.
- **Codex:** codex-09b (ends 22 Oct).
- **Cloud:** reserve only ($52).
- **Grok bots:** grok-09.

## #50 CLEARED (20 Oct 19:15): rb-01b checked, merge it
Head `6bb935c`, 5 new commits, all in scope, no `packages/` changes, adapter tests 38/38 (chat re-ran them). The smoke test now fails on any non-ok status and on zero total findings. Image `sha256:95cc69d2…`, anonymous pull works. The builder corrected the brief: the cap is crossed by the verifier's 4 concurrent calls, not by 2 reviewer calls. **The 3.5 id is still unconfirmed:** smoke run 3 (pool-raw, 3.5) is the confirmation, because a wrong id now turns the run red.

### Earlier: HOLD #50 (20 Oct 18:55)
Checked by chat on head `1d06203`:
- 27/27 adapter tests pass on Node 22.
- The never-fail wrapper, the 40-finding cap and the contract schema check are fine.
- `golden/` is banned in the docs.
- The image workflow run #1 is green. **Sunny:** read its summary for the digest and whether the package is public.

Gaps:
1. **No 3.5 price.** The $5/$15 placeholder × 32k `max_tokens` breaks the $1 per-process cap, so every 3.5 call is refused and the run silently scores 0.
2. **The smoke test can't fail.** The wrapper always exits 0.
3. The smoke test has no model input.

Fix: [rb-01b](prompts/2026-10-20/rb-01b-nemotron-3.5.md), in the same cloud session as #50.

**P008 amendment 2 (decided by the chat, Sunny delegated):** candidate 6 is `pool-raw-missed`, built by [rb-02](prompts/2026-10-20/rb-02-missed-issue-pass.md) (new cloud session, after #50 merges). Cutoff 23 Oct 12:00 IST.
- **No ReviewBench test run of any candidate starts until the chat has recorded rb-02's prompt sha256 in P008.**
- If rb-02 misses the cutoff, drop candidate 6 and start the runs.

## grok-08 (20 Oct 18:20)
- Filed → [R11](research/R11-grok08.md). Kill test 5/5 real, 3 overstated. **No P008 amendment**: its top 3 are `pool-raw-x2` (already candidate 3) or new prompts/code. Adds one rb-01b check: 3.5 smoke must report `finish_reason` (reasoning shares `max_tokens`). **ChatGPT pass (18:35):** kill test 5/5 (LAURA misattributed to callers/callees; Qodo extended numbers only in charts). New: **missed-issue pass** (CR-Bench: +5.75 recall on GPT-5.2, closed models, iterative). Chat recommends **no** candidate 6; **Sunny decides before any test run**. rb-01b also checks `learn` off in the image.

## ReviewBench (19 Oct 18:20): the main lever toward "above Greptile and Cursor"
- 219 PRs, run by GitHub + Microsoft; grounded F1 is driven by recall; Greptile 27.2, Cursor 17.3, Copilot 40.1 ([R7](research/R7-cc02-reviewbench.md), [R9](research/R9-reviewbench-methodology.txt)).
- **Pre-registered: [P008](decisions/P008-reviewbench-preregistration.md).** Never open `golden/`. Choose between pool-verify-v1, pool-raw and pool-raw-x2 from the 25-PR test runs only; then one final run.
- Prompt `rb-01-reviewbench-image.md` (cloud session): adapter, Dockerfile, ghcr build, smoke workflow. Then Sunny: portal registration → approval → 3 test runs → final.
- **Nemotron 3.5 Super is on Token Factory at the same price** as 3 Super (SWE-bench Verified 71.1 vs 60.5); see [R10](research/R10-smarter-reviewer.md). It was added to the ReviewBench candidates (P008 amendment 1) and to rb-01's support list. Fine-tuning isn't possible: Token Factory post-training has no Nemotron.
- Needs a Nebius top-up of about $25. Sunny has Nebius **sandbox** access (beta); it's not needed for ReviewBench, but it's a candidate for "checked fixes" later.

## Prompt queue (19 Oct 03:45)

| Lane | File (prompts/2026-10-19/) | Where | Status |
|---|---|---|---|
| Codex | `codex-08b-gate-fixes.md` | the **same** Codex task as #45 | ready |
| ~~Relay~~ | ~~065~~ → **#48, a loss** | | done |
| Relay | `066-results-docs.md` (R6 fixes, all-50 with a dev 5-run-mean row, levers table, README headline; **$0**) | `~/Projects/tracehound-ops` → TraceHound | after #48 merges; Sunny sets `nebiusCapUsd` = 0 |
| Codex | `codex-09-learning.md` (S8: 👎 / resolved-unchanged / reply → suppress; maintainers only; `learn` input default off; conventions reader, not wired) | ChatGPT Codex, `~/Projects/TraceHound-codex`, new task | **ready, must land by 22 Oct** |
| (old) Relay | `065-evidence-gate.md` | Run 1 stopped at step 0 at 13:00 IST ($0). Brief patched. **#45 is merged now, so it's ready to rerun**: re-copy the brief from the brain, then start. lanes.json = 5 (confirmed by relay) |
| ~~Grok~~ | ~~grok-07~~ → [R8](research/R8-competitors-learning.md): links recovered and 5 of 5 checked. **Sunny keeps CodeRabbit** (seeded test + public numbers) despite §4.2(iv) of its terms. Seeded-test tools: TraceHound, CodeRabbit, Greptile, Bugbot (Bugbot ≈ $1–1.5 per run) | | done 19 Oct |
| Cloud | ~~cc-04 + cc-04b~~ **done 17:40**: `tracehound-seeds` `a4a6c83`. Scorer has 13/13 offline checks; `open-prs.sh` triggers CodeRabbit and opens one PR per 450 s (≈3 h 40 m for 30). **Pre-registration (published 19 Oct 17:50 IST):** `sha256(key.json) = abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1`, `sha256(SCORING.md) = ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d`. 25 Oct: install the TraceHound Action (freeze-2 pin), CodeRabbit, Greptile and Bugbot on both mirrors, then Sunny runs `open-prs.sh` | | done |
| Codex | codex-09 learning (S8) | — | after codex-08b |
| Claude B | int-01 integration (`th-v2` in the Action) | `~/Projects/TraceHound-claude2` | 22 Oct |

## Prompt queue (18 Oct 21:45). Every lane busy, no path overlaps

| Lane | File | Where | Waits on |
|---|---|---|---|
| Relay | `064-vote.md` (voting across runs, dev only, cap $10) | `~/Projects/tracehound-ops` → TraceHound | `nebiusCapUsd` = 10 |
| ~~codex-07~~ → **#43** | | |
| ~~ui-02~~ → **#44** | | |
| ~~cc-01~~ → **#41** draft, [R6](research/R6-cc01-audit.md): numbers all correct; 5 wording overclaims in results.md; gaps within noise except CodeRabbit | | |
| ~~cc-02~~ → **#42** draft, [R7](research/R7-cc02-reviewbench.md): probably feasible; 25-PR practice set ≈ $4; judge needs Anthropic API money | | |
| Grok | ~~grok-05~~ → [R4](research/R4-precision-techniques.md) (done 18 Oct; Cursor's early Bugbot = 8-pass vote + validator) | | |
| Grok | ~~grok-06~~ → [R5](research/R5-hackathon-and-nebius-levers.md) (done 18 Oct; Coding track favours plan → test → iterate agents; weights unknown) | | |

Path guard: relay owns `scripts/bench/`, `docs/bench/` except the cc-01/cc-02 files, and `docs/decisions.md`; Codex owns `packages/action/`, `.github/`, `docs/action/`; UI owns `viewer/`, `docs/ui/`; cc-02 owns `scripts/reviewbench/`.

## Credits
- Nebius console $39.93 (18 Oct 21:00).
- **Ultrareview: 3 free runs** (Claude cloud bug-finding agent, about 15–25 min each). Spend them only on branches before merge, highest risk first:
  1. codex-07 branch (adds `contents: write` and pushes the orphan branch; touches other people's repos);
  2. 064 branch (voting and scoring code that the headline number depends on);
  3. reserve: the final pre-submission branch (27–28 Oct), or ui-02 if it renders untrusted comment text.
  Never on main, and never on docs-only PRs.
- Claude cloud: **$89 of $100 left, expires 5 Nov 13:29 IST**. It's cloud-session credit only (screenshot 18 Oct), **not API credit**, so it can't pay the ReviewBench judge (Claude Sonnet 5 API). Judge cost is about $3 for the 25-PR practice set and about $7 per 60-PR run; that needs a separate Anthropic API top-up (Sunny's call).
- (Superseded line follows.) Claude cloud $96, expires 5 Nov. Spend it on audits, spikes, judges' README drafts and TariffLens setup after 29 Oct.

## Honest claim status (18 Oct, revised after cc-01)
- **Public line:** "on 10 blind PRs, statistically tied with Greptile and Cursor, ahead of CodeRabbit; 4th of 6 across all 50 PRs; every loss published." Don't say "3rd of 6" as a ranking.
- Blind Cal.com: 3rd of 6 (10 PRs, n = 23).
- All 50 PRs: **4th of 6** (41.2 vs Greptile 49.5, CodeRabbit 40.0).
- Sunny does not want claims that draw "how did you beat CodeRabbit?" questions, so the post wording is "3rd of 6 blind, 4th of 6 across all 50, losses published".
- The top-2 push (064) is judged on all 50 PRs, not on Cal.com alone.
