# TraceHound handoff, 23 Oct 2026, 16:40 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer (GitHub Action) on NVIDIA Nemotron via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**. Deadline 30 Oct 2026, 22:30 IST; the target is to submit on 29 Oct.
- **You plan; you don't build.**
  - Every prompt is a file in the brain (`prompts/<date>/<job>.md`), pushed before Sunny pastes it.
  - Always say which tool, folder and session it goes in, and send the file with SendUserFile.
  - Give Sunny step-by-step instructions for anything done by hand (GitHub UI, portal, terminal). He asks for them.
- **Check every PR from GitHub before Sunny merges it.**
  - Fetch `refs/pull/N/head` into a read-only clone of the public TraceHound repo (`add_repo` access read).
  - Read the diff and recompute tables.
  - Probe deterministic logic with adversarial inputs.
  - Run the relevant tests on Node 22 with vitest. For the ReviewBench adapter that's `pnpm exec vitest run --config scripts/reviewbench/vitest.config.ts` and `pnpm exec tsc -p scripts/reviewbench`.
  - Put **HOLD #N** first when something must not merge.
  - Keep checks lean.
  - **Also check where the measured config and the product differ** (lessons, 22 Oct).
- **Brain:** `SunnyBagal/tracehound-brain`; attach it with `add_repo` access push.
  - **Process `inbox/` first.** `git mv` each builder report to `prompts/<date>/<job>.out.md`.
  - File every agent output verbatim.
  - Update NOW, prs, timeline and lessons after each event, then push.
  - The brain is public: no secrets, no Cal.com comment text.
- **Style:** short, blunt, honest. Answer what Sunny asks. Don't build unrequested deliverables; offer extras in one line.

## Read in order
1. `NOW.md`: the top sections are current.
2. `decisions/P008-reviewbench-preregistration.md`: amendments 1–4, plus 5, which is pending (below).
3. `research/R12`–`R14`. Correction to R12: **ReviewBench pays the judge on test runs too.** The credentials form asked only for `NEBIUS_API_KEY`.
4. `lessons.md`, top 4 entries.
5. `inbox/README.md`.

## Lanes (names are Sunny's)

| Lane | What it is | Folder | State |
|---|---|---|---|
| **Sunny** (by hand) | merges, GitHub Actions UI, ReviewBench portal, console | — | see "Pending with Sunny" |
| **Relay** | Sunny's Claude Code, account A. Run in a **plain terminal**, not inside `claude`: `cd ~/Projects/tracehound-ops && caffeinate -i bun src/relay.ts start tracehound briefs/<file>.md` (copy the brief into `briefs/` first) | `~/Projects/tracehound-ops` → `~/Projects/TraceHound` | 067b done (#56). Idle. `nebiusCapUsd` should be **0** until the next brief (Sunny to confirm). Ignore relay's own proposed 068 prompt |
| **Bunny** | second Claude Pro account. `cd ~/Projects/TraceHound-claude2 && claudeb`. No `.env`, so no Nebius calls | `~/Projects/TraceHound-claude2` | bunny-03 done (#55). **Idle.** Next: README fill (26 Oct), Devpost docs |
| **Codex** | ChatGPT Codex. **Extended:** Sunny has ChatGPT Plus for 1 month | `~/Projects/TraceHound-codex` | codex-12 done (#57). Idle. Optional: a read-only audit of contracts against hostile PR files (tsconfig `extends`/`plugins`, symlinks), which nobody has covered |
| **Claude cloud** | claude.ai/code. **1 Ultrareview left** (reserve) | — | — |
| **Grok bots** | web only. Researcher → Verifier; you link-check | — | Verifier B (post drafts) waits until about 25–26 Oct, when the numbers are final |

## State (verified 23 Oct 16:40)
- **TraceHound main `8d0fb7a`** (#56 merged). Merged since 20 Oct:
  - **#52** (066 docs);
  - **#53** (int-01 contracts in the Action, default on; merge `7de5dbd`; Ultrareview #2's 7 findings fixed by codex-11);
  - **#55** (per-model base URL; merge `da13585`);
  - **#56** (067: 3.5 adopted on dev).

  #42 is closed.
- **Open:**
  - **#57** (codex-12, decision 068, head `4e3a0a8`). **Checked by chat: cleared to merge.** Action 237/237, adapter 70/70, adapter tsc clean. Partial results when some reviewer chunks fail. Nothing is posted only if both reviewers fail every chunk. The footer gets `chunkFailed`. The body says "this review is partial". Byte-identical when nothing fails. The `model` input defaults to `nvidia/Nemotron-3.5-Super-VL`; fixes stay on 3 Super (`FIX_MODEL`). Smoke accepts `status=ok partial=N/M`.
  - **#54** (Bunny's judges' README, `db83286`): **stays a draft** until the 26 Oct fill. When filling:
    - the try-it pin goes to `review-freeze-2`;
    - drop its `model:` line, because the default is now 3.5;
    - drop "landing in int-01";
    - remove the duplicate `# TraceHound` heading;
    - check the Mermaid render.
- **Nemotron 3.5 Super VL** (`nvidia/Nemotron-3.5-Super-VL`):
  - callable **only on us-north1**; us-central1 lists it but answers 404;
  - 3 Super exists **only on us-central1**;
  - the client routes per model (#55, `baseUrlFor`).
- **067 result (2 dev runs; the cap cut run 9):**

  | | 3 Super (5 runs) | 3.5 (2 runs) |
  |---|---|---|
  | v1 F | 42.3 | 50.4 |
  | v1 P | 40.5 | 54.8 |
  | x1 F | 51.3 | 55.1 |

  - Bootstrap: v1 +8.1 [+1.4, +14.4]; x1 +3.9 [−4.2, +11.1].
  - Cost per PR: median $0.063 against $0.031.
  - Invalid JSON on 7/40 PRs per run.
  - 79/624 calls hit `max_tokens` 32,000.
  - Public wording must say "2 dev runs" and that the x1 interval includes 0.
- **Ledger:** $33.81 of $45 (local; it excludes CI and ReviewBench spend). **Sunny will top up about $50** on the Nebius console (decided 23 Oct).
- **ReviewBench:**
  - Registered as reviewer `tracehound`. **Onboarding PR review-bench/ReviewBench#92**: its approval state is unknown, so ask.
  - Model API URL us-central1; egress us-central1 + **us-north1**.
  - Separate Nebius key `reviewbench`; credentials are per configuration.
  - Configs: candidate 2 (`pool-raw`) and candidate 1 (`pool-verify-v1`), both 3 Super, on the **old image `ff22b972`** (all-or-nothing). **They need re-adding on the post-#57 image.**
  - Labels must be exactly `pipeline=…` and `model=…`. Delete the form's pre-filled keys.
  - **Never** click "Register a reviewer" again; use "+ Add a configuration".
  - Images:
    - `ff22b972` (main `2a95756`);
    - `8e1ec5f` (`7de5dbd`, no routing, unused);
    - `bc6530f2` (`da13585`, routing, all-or-nothing);
    - the next build after #57 is the one to register.
  - Smoke results:
    - 3 Super: pool-raw, pool-verify-v1 and pool-raw-missed all green;
    - 3.5 pool-raw: red, idx 0 invalid JSON (pre-#57).
  - **Never open `golden/`.**
- **P008 amendment 5: PROPOSED, Sunny hasn't answered clearly.** All candidates run on the post-#57 image (partial results, the same rule for every candidate). It must be written to P008 **before any test run**. Amendment 3's fallback is moot (the judge is paid). Amendment 4 is moot (3.5 is callable).
- **Demo/dogfood:** both workflows don't set `model`, and both are pinned to old shas (demo main `41a92a8`, dogfood `e4e3e3a`), so they run Nano generators. Re-pinning to the #57 merge fixes it, since the default becomes 3.5. **Demo PR #6 must never be merged.**
- **Public claim wording (066):** "On 10 blind PRs, statistically tied with Greptile and Cursor; run-1 F 58.3 vs CodeRabbit 36.7, but not a significant lead (the 95% interval includes 0; ahead in 96% of resamples). 4th of 6 across all 50 under both judges. Every loss is published." **Never "3rd of 6" as a ranking, never "beats".**
- **Devpost (R14):**
  - no competitor names or logos in the video;
  - the demo must work without a judge's key;
  - MIT LICENSE;
  - FEEDBACK.md;
  - "what changed since 26 Aug".
- **Tool terms (R13):** Greptile §1.4(a) needs consent to publish any results, so Sunny must email them. Cursor needs a replication kit. CodeRabbit is Sunny's call (kept).
- **Blindness:** no lane opens `linea-seeded`, `recall-seeded` or `tracehound-seeds` until scoring on 25 Oct. Cal.com stays blind except in pre-registered runs. No ReviewBench `golden/`.

## Pending with Sunny
(Sunny was unwell on 22 Oct and did none of the hand steps that day. Relay's 3 questions, relay's `nebiusCapUsd` = 0 and the top-up are all still open. If freeze-2 can't happen on 24 Oct, it may slip to 25 Oct. Everything after it shifts one day, which uses the 30 Oct buffer. Submit on 29 Oct stays the target.)

1. **Merge #57**: "Ready for review", then merge with a merge commit. Then send the merge sha.
2. **P008 amendment 5:** yes or no.
3. **After #57 merges:**
   - rebuild the ReviewBench image on main;
   - run smoke `pool-raw` on **3.5** (expect `status=ok` or `ok partial=…`);
   - add the portal configs on the new digest (exact labels, then store credentials).
4. Check whether ReviewBench #92 is approved.
5. Top up the Nebius console by about $50.
6. Confirm relay's `nebiusCapUsd` = 0.
7. Email Greptile for consent.
8. **24 Oct freeze-2:**
   - tag `review-freeze-2` on the #57 merge commit;
   - re-pin the demo `main` and dogfood workflows to it.

   (`eval-freeze-2` already exists, from 4 Oct; don't reuse the name.)

## Next jobs to write
1. A **freeze-2 checklist** for Sunny (tag, re-pin; exact clicks and commands).
2. **The v2 blind Cal.com run brief** (relay, its own Nebius cap; 3.5 costs about 2×). Pre-register it like 061.
3. **The seeded test (25 Oct):**
   - install the Action at the freeze-2 pin, plus CodeRabbit, Greptile and Bugbot, on both mirrors;
   - Sunny runs `open-prs.sh`;
   - scoring uses the private key `a4a6c83`, with hashes in NOW.
4. **The ReviewBench test-run order under P008** (6 candidates). Then selection → the one final run (permanent).
5. **Bunny:** fill the README placeholders (26 Oct), Devpost docs ("what changed since 26 Aug", the FEEDBACK summary). Then the Grok final audit (26 Oct), the Devpost write-up, and the video (no competitor names). Submit 29 Oct.
6. **Backlog:**
   - the contracts analyzer budget covers only the typed check, so keyed and dependents are unbounded (an analyzer change);
   - the merge-base fetch code is duplicated in `data.ts`;
   - the hostile-PR audit.

## Side project
TariffLens starts 30 Oct.
