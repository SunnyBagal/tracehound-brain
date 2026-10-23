# TraceHound handoff, 24 Oct 2026, 02:35 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer: a GitHub Action on NVIDIA Nemotron via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**.
  - Deadline: 30 Oct 2026, 22:30 IST. Target: submit on **29 Oct**.
  - **Sunny is away 24 Oct 18:00 → 25 Oct 18:00 IST** (HackerRank Orchestrate).
- **You plan; you don't build.**
  - Every prompt is a file in the brain (`prompts/<date>/<job>.md`), pushed before Sunny pastes it.
  - Say which tool, folder and session it goes in, and send the file with SendUserFile.
  - Give Sunny step-by-step instructions for anything done by hand.
  - **URLs go on their own line, as full `https://` links, with no trailing punctuation** (lesson, 23 Oct).
- **Check every PR from GitHub before Sunny merges it.**
  - Fetch `refs/pull/N/head` into a read-only clone of the public TraceHound repo (`add_repo` access read).
  - Read the diff, recompute tables, probe deterministic logic, and run the relevant vitest files on Node 22.
  - 5 s test timeouts under load are known flakes: rerun that file with `--testTimeout 30000`.
  - Put **HOLD #N** first when something must not merge.
  - Also check where the measured config and the product differ.
- **Brain:** `SunnyBagal/tracehound-brain` (`add_repo` access push).
  - Process `inbox/` first: `git mv` each report to `prompts/<date>/<job>.out.md`.
  - Update NOW, prs, timeline and lessons after each event.
  - The brain is public: no secrets, no Cal.com text.
- **Style:** short, blunt, honest. Answer what Sunny asks; offer extras in one line.

## Read in order
1. `NOW.md`: the top sections (24 Oct 02:30, back to 23 Oct 17:15) are current.
2. `decisions/P008-reviewbench-preregistration.md`: amendments 5 (partial results, image `9b9a5127`), 6 (report-only Ultra row) and 7 (the one final may go to v3).
3. `decisions/P009-reviewbench-v3-recall.md`: the v3 recall-mode pre-registration.
4. `research/R16-reviewbench-grounded-f1.md` (what raises grounded F1) and `research/R15-ui-references.md` (UI).
5. `lessons.md`, the top 3 entries.

## Lanes

| Lane | What | Folder | State |
|---|---|---|---|
| **Sunny** (by hand) | merges, Actions UI, ReviewBench portal, console | — | see "Pending with Sunny" |
| **Relay** | Claude Code account A. Run in a **plain terminal**: `cd ~/Projects/tracehound-ops && caffeinate -i bun src/relay.ts start tracehound briefs/<file>.md` | `~/Projects/TraceHound` | **Running [070](../prompts/2026-10-23/070-v3-recall-mode.md)** (build recall-v3 + dev arms S/V). It hit the session limit once and was restarted about 02:30. `nebiusCapUsd` 14, local total cap **$60**. Verdict due 24 Oct 12:00 → inbox `2026-10-2x-sunny-070.md` |
| **Bunny** | Claude Pro account B, `cd ~/Projects/TraceHound-claude2 && claudeb`. No `.env` | `~/Projects/TraceHound-claude2` | Idle. bunny-04 (#58) and bunny-05 (#54, #61, #58) done. Next: the 26 Oct fill |
| **Codex** | ChatGPT Codex (Plus until about 20 Nov) | `~/Projects/TraceHound-codex` | Idle. **OpenAI's cyber filter blocks security-audit prompts**: don't give it audits |
| **Cursor** | Sunny's Cursor premium (new 23 Oct) | — | Planned: **Bugbot** in the seeded test and as an extra reviewer on the 070 PR; a Cursor agent as the **UI builder, `viewer/` only** |
| **UI chat** | a separate chat session for the frontend redesign (major on the frontend, small on the graph page) | — | Sunny runs it. It should use R15 and keep `/benchmark` = `results.md` (viewer test). Merge by 27 Oct noon |
| **Grok bots** | web Researcher → Verifier | — | grok-11 done (R15). Verifier B (post drafts) at 25–26 Oct |
| **Claude cloud** | 1 Ultrareview left (reserve) | — | — |

## State (verified 24 Oct 02:35)
- **TraceHound main `f488cef`** (#61 merge). Merged on 23 Oct:
  - **#57** (`0aeef06`: 068, 3.5 default + partial reviews);
  - dogfood re-pin `8712273`;
  - **#59** (`5fb8cdc`: 069);
  - **#60** (`0c20234`: 069b);
  - **#61** (`f488cef`: contracts security-boundary docs).
- **Freeze-2:** tag **`review-freeze-2` → `0aeef06`** (tag object `6d38b35`).
  - Dogfood (TraceHound `8712273`) and demo (tracehound-demo `88f6721`) are pinned to `@0aeef06…` with `model: 'nvidia/Nemotron-3.5-Super-VL'`.
  - Demo PR #2 on 3.5 posted **no findings** (the old Nano generators had caught its seed). The call was no change, and no more reopening of demo PRs.
  - **Demo PR #6 must never be merged.**
- **Open PRs (drafts):**
  - **#54** judges' README (`1c57c72`): up to date except the numbers (`[[RESULTS-TABLE]]`, `[[REVIEWBENCH]]`, `[[SEEDED-TEST]]`, `[[COST]]`, `[[CALCOM-V2]]`).
  - **#58** Devpost drafts (`ff483fc`): 12 `[[SUNNY]]` questions in the report. Fill on 26–27 Oct.
- **069 v2 blind Cal.com** (second look, one run): pool-verify-v1 on 3.5, v1 **P 63.2 / R 52.2 / F 57.1**, x1 F 58.5 (3 Super run 1: 58.3). All 3 predictions right.
- **069b intervals** (PR-level paired bootstrap, seed 69). The **allowed sentences:**
  - all 50 PRs: **behind Augment; statistically tied with Cursor and Greptile; ahead of Copilot and CodeRabbit** (both judges);
  - Cal.com: tied with all except CodeRabbit (ahead; a 10-cluster caveat);
  - "3rd of 6" only as a point estimate.
  - Earlier claim (066, 3 Super): still valid for that row.
- **ReviewBench** (reviewer `tracehound`, approved; onboarding #92 merged):
  - all configs on image **`ghcr.io/sunnybagal/tracehound-reviewbench@sha256:9b9a5127317be79f4bc9f7e771b84b441d887f5cdd52e38e934b8657ceb84508`** (from `0aeef06`);
  - smokes green: pool-raw/3.5, pool-verify-v1/3.5 (`ok partial=1/2`), pool-raw-x2/3 Super.
  - **Test runs (25 PRs; grounded F1 / P / R · augmented F1 / P):**
    - **#331 cand 3 pool-raw-x2 · 3 Super: 21.3 / 76.3 / 12.4 · 30.6 / 50.6, 222 findings** ← the current P008 winner;
    - #328 cand 2 pool-raw · 3 Super: 16.0 / 70.6 / 9.0 · 23.0 / 52.5;
    - #330 cand 1 pool-verify-v1 · 3 Super: 13.9 / 79.5 / 7.6 · 22.3 / 67.2;
    - #329 cand 5 pool-raw · 3.5: 13.5 / 56.3 / 7.7 · 19.0 / 44.0;
    - #332 cand 4 pool-verify-v1 · 3.5: 12.2 / 69.2 / 6.7 · 17.6 / 57.2.
  - **Candidate 6 = pool-raw-missed on 3 Super** (cand 2 beat cand 5 by 2.5). It needs a smoke on `9b9a5127` first (its only green smoke was on `ff22b972`), then a portal config, then a test run. **It must reach ≥ 24.3 to win.**
  - Leaderboard context (219 × 3): Greptile 27.2, Cursor 17.3, Copilot 40.1, leaders 48–60; everyone's augmented P is 80–92 (ours is about 50).
  - **Never** click "Register a reviewer"; use "+ Add a configuration". Labels are free text: delete the pre-filled keys, then exactly `pipeline=…` and `model=…`. **Never open `golden/`.** One test run per config.
- **v3 (P009, relay 070):** recall-v3 = per-file chunks, lens A + lens B, `review-missed-v1`, T 1.0 / top_p 0.95, reasoning budget, anchor/dedup/cap 60.
  - Dev arms S (3 Super) and V (3.5). Floor: v1 P ≥ same-model pool-raw dev mean − 10. Pick: higher v1 recall.
  - Then a new image → smoke → one test run. **The final goes to v3 only if its test grounded F1 ≥ the P008 winner + 3.0 (now 24.3).**
  - Cutoff: v3's test run by 25 Oct 23:59.
  - Disclosure: designed after seeing candidate 2's aggregate.
- **Money:** Token Factory console **$68.09** (02:26). Local ledger about $34.5 of a $60 cap. Still to pay:
  - candidate 6, about $2.5;
  - relay 070, ≤ $14;
  - v3 smoke + test, about $3;
  - **one final**, about $33–40 (x2 is unmeasured; the v3 estimate gets replaced by 070's measured cost).
  - It fits, with little spare; Ultra (amendment 6) likely won't. Top up only if measured costs come out higher.
- **Codex-13 audit (partial):** a PR's tsconfig `extends`/`paths` can make the compiler read files outside the checkout. No PR JS executes. That's within the trust boundary (same-repo PRs only; forks skipped). Docs fixed in #61. Hardening is backlog.
- **Public wording rules:**
  - never "beats";
  - "3rd of 6" only as a point estimate;
  - 3.5 on dev is "2 dev runs";
  - **no competitor names or logos in the video or the Devpost text** (R14);
  - "ahead/behind" only where 069b's interval excludes 0.

## Pending with Sunny
1. **Candidate 6 smoke** (see below), then its portal config and test run. Send the summaries.
2. Send relay 070's report when it lands (by 12:00).
3. Before 18:00: start the v3 smoke and test run if 070 qualifies, and start **the final** if its config is decided. The final needs no babysitting.
4. Greptile consent email (R13). Chat can draft it in Gmail.
5. Enable Bugbot on `SunnyBagal/TraceHound` (for the 070 PR review) and on the seeded mirrors (26 Oct).
6. UI chat: spec → Cursor builds `viewer/` → merged by 27 Oct noon.

## Next jobs for the planning chat
1. Check 070's PR (a new pipeline, additive only; existing pipelines byte-identical; prompt shas recorded before calls; the dev table recomputed). Then the v3 image rebuild + smoke instructions.
2. Apply the selection rules: P008 (cand 6 vs 3) → P009/amendment 7 (v3 vs the winner + 3.0). Then the final-run instructions.
3. **The seeded-test brief (26 Oct):**
   - TraceHound at `@review-freeze-2` (`0aeef06`) with the explicit 3.5 model, plus CodeRabbit, Greptile and Bugbot, on `linea-seeded` and `recall-seeded`;
   - Sunny runs `open-prs.sh`;
   - scoring with private key `a4a6c83`;
   - blindness until scoring.
4. **The 26 Oct fill** (one job, after all numbers):
   - results.md + viewer + README (#54) + Devpost (#58) with 069, 069b, ReviewBench final, seeded test;
   - the "no network" wording ("no model calls; network only for a credential-free git fetch of the base commit");
   - CLAUDE.md's "read-only symlinks".
   - Coordinate with the UI branch.
5. Bunny: Devpost + video script fill (27 Oct); Grok Verifier B (post drafts).
6. **Ultra on build.nvidia.com (free, 40 RPM):** a dev-only, report-only row; brief after 070 merges (see NOW 03:05).
7. **Backlog:**
   - the no-post log line should carry pool/kept counts;
   - the contracts budget covers only the typed check;
   - compiler-read containment + env allowlist;
   - truncation (`max_tokens`) for the Action;
   - the merge-base fetch duplicated in `data.ts`.

## Side project
TariffLens starts 30 Oct.
