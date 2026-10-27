# TraceHound handoff, 27 Oct 2026, 18:00 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer (GitHub Action on NVIDIA Nemotron via Nebius Token Factory) plus a repo component graph, for the **Nebius × NVIDIA hackathon**. **Deadline 30 Oct 22:30 IST; target submit 29 Oct.** The video is recorded **28 Oct afternoon**.
- **You plan; you don't build.** Every prompt is a file in the brain (`prompts/<date>/<job>.md`), pushed before Sunny pastes it. Say which tool, account, folder and session it goes in; send it with SendUserFile. Give step-by-step instructions for anything by hand. URLs on their own line, full `https://`, no trailing punctuation.
- **Check every PR from GitHub before Sunny merges** (read-only clone of the public TraceHound repo via `add_repo` access read; fetch `refs/pull/N/head`; read the diff; recompute tables; run the relevant vitest files on Node 22; 5 s timeouts are known flakes → rerun with `--testTimeout 30000`). **HOLD #N** first if something must not merge.
- **Brain:** `SunnyBagal/tracehound-brain` (`add_repo` access push). Process `inbox/` first (`git mv` to `prompts/<date>/<job>.out.md`). Update NOW after each event. Public: no secrets.
- **Single source of truth for every published number and claim: `submission/numbers.md`.** Its §0 wording rules are hard (never "beats"; "ahead/behind" only where 069b's interval excludes 0; "3rd of 6" only as a point estimate; **no competitor names or logos in the Devpost text or video, and don't name Cursor there even as a tool**).
- **Style:** short, blunt, honest. Sunny merges with "Create a merge commit": just say "commit".
- **Correct Sunny when he overclaims** (27 Oct he said "we beat Greptile": it's "statistically tied").

## Read in order
1. `submission/numbers.md` (all; only open slot: §7 seeded PRs).
2. `NOW.md`: entries 27 Oct 17:45, 02:45 (lane board), 01:50.
3. `prompts/2026-10-27/` briefs: `ui-11-ui-v2-claude-b.md`, `seed-01-sunny-and-scoring.md`, and the `.out.md` reports.
4. `lessons.md` top 3.

## State (27 Oct 18:00)
- **TraceHound main `36de717`** (#62 merged). Product frozen: **pool-verify-v1 on `nvidia/Nemotron-3.5-Super-VL` at tag `review-freeze-2` (`0aeef06`)**. Nothing changes the product before submission (decided; improvements ship as report-only + "what's next").
- **PRs:**
  - **#63** (071 Ultra, head `b5a5f2e`, CI green): checked by chat 27 Oct (scope 9 files, no product paths; llm tests 55/55). **OK to merge.** Ultra 2-run: pool-verify v1 F 47.1 vs 3.5 50.4 (no interval-backed difference); pool-raw worse than 3.5 (−10.2 [−17.7, −3.2]). Devin's 4 findings + Bugbot's root cause fixed. Leftover: 2 dogfood comments on `scripts/bench/ultra-smoke.ts` (lines 26, 51) → backlog, not fixing.
  - **#64** (codex-14 levers v2, head `a149e17`, $0 offline replay, 4 new files only, plan committed before compute): **OK to merge** after CI. Results are in numbers.md §9.
  - **#54** README (`docs/judges-readme`, head `64082a7`) and **#58** Devpost (`docs/devpost-drafts`, head `8184e54`): filled by bunny-06 from numbers.md. Open: `[[TBD-SEEDED]]` (×several), `[[TBD-ULTRA]]` and `[[TBD-LEVERS]]` (now fillable from numbers.md §5/§9), five `[[SUNNY]]` (Q1 personal reason, Q5 YouTube URL, Q6/Q7 what existed before 26 Aug / 30 Sep, Q10 Vercel until 15 Dec). **Chat must do the claims check** (Gemini dropped): every number vs numbers.md, incl. bunny-04 leftovers not in numbers.md (79 of 624 calls at the token limit, 1,305 tests, $0.0113 ledger gap, 26 FEEDBACK entries) → verify in repo docs or cut. Drop "Cursor" from the Devpost "how we built it" list.
- **Running / next lanes:**
  | Lane | Job | State |
  |---|---|---|
  | Relay (account A, `~/Projects/TraceHound`) | 071b | done → BLOCKED waiting for Sunny to merge #63 |
  | Account A next | **the fill**: `docs/bench/results.md` + `viewer/lib/benchmark.ts` (they must match; `viewer/test/benchmark.test.tsx`) with 069/069b/ReviewBench/070/071/levers; then fill the README/Devpost `[[TBD-…]]`; **coordinate with ui-11** (ui-11 must not edit `benchmark.ts`; the fill must not touch other viewer files) | brief **not written yet** |
  | Account A after | seed-01 part B (scoring in a `tracehound-seeds` clone) | waits for Sunny's part A |
  | Account B (`claudeb`, `~/Projects/TraceHound-ui`, branch `ui/v2`) | **ui-11**: expand-bug fix → PR lane on the graph → findings pinned to components → redesign (Vite/Vite+ inspired, Sunny's own hero design) → Recall snapshot at `62eaca1` | to start; due **28 Oct 12:00** |
  | Sunny by hand | seed-01 part A: workflow + `NEBIUS_API_KEY` secret on `linea-seeded`/`recall-seeded`, optional Bugbot, `open-prs.sh` without the CodeRabbit trigger | to do |
  | Codex | done (#64) | idle |
  | Devin | reviewer on every PR | on |
  | Gemini | research only (hallucinates on build tasks) | — |
- **After seed-01 scoring:** flip ui-11's `recall-seeded` mapping flag on, fill numbers.md §7, then README/Devpost/video `[[TBD-SEEDED]]`.
- **Relay's open questions (answered):** merge #63 yes (Devin's duplicate-token thread already fixed in `b5a5f2e`); ultra-smoke dogfood comments → backlog (note: the suggestion on lines 25–26 would break the script, since `apiKey` feeds `buildClients`). `nebiusCapUsd` = 0 (Sunny set it 27 Oct). **There is no total cap in `lanes.json`**: the $45 is the code default `TRACEHOUND_BUDGET_TOTAL_USD`, overridden per command when needed; no remaining job needs it (seed-01 reviews run in GitHub Actions, off the local ledger).
- **Money:** Nebius balance **$43.52** (24 Oct; minus ~$0.2 judges since). No top-up (Sunny). Estimates always ×1.18 VAT. Remaining uses: seed-01 (~$3), dogfood. No paid experiments.
- **Recall** head is now `62eaca1` (Sunny deleted orphan `SidebarToggle.tsx`; `Usecontent.tsx` is still an orphan, deliberately kept). Snapshot regenerate either by Sunny in `~/Projects/TraceHound-snap` (instructions given 27 Oct 15:55) or ui-11 task 2.
- **Live demo problem:** demo PR #2 posts no findings under 3.5; older demo reviews are Nano generators (never present as current). The seeded Recall PRs on the graph are the demo answer.
- **Not doing:** a new demo project with planted bugs (credibility); live graph sync on push (roadmap); a ReviewBench final (~$107 with VAT; P008 amendment 8).

## Schedule
- **27 Oct (rest):** merge #63, #64 → write + run the fill (account A) → ui-11 running (account B) → Sunny does seed-01 part A.
- **28 Oct:** ui-11 PR by 12:00 → chat checks → merge → seed scoring → fill seeded slots → Sunny answers the 5 `[[SUNNY]]` questions → chat claims check on #54/#58 → merge → **record the video** (script: `docs/devpost/video-script.md`, ≤ 3:00).
- **29 Oct:** Devpost submit (YouTube URL in). Repo About text fixed (still pre-pivot: "Persistent repository intelligence…"). 30 Oct: buffer only.

## Backlog (post-hackathon)
JSON-repair / schema-constrained output for truncated chunks; per-PR cap on the shipped pipeline (no evidence yet); ultra-smoke comments; no-post log line with pool/kept counts; contracts compiler-read containment; truncation for the Action; merge-base fetch duplicated in `data.ts`; live graph sync (webhooks + queue); TariffLens starts 30 Oct.
