# TraceHound handoff, 20 Oct 2026, 18:00 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer on NVIDIA Nemotron via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**. The deadline is 30 Oct 2026, 22:30 IST; the target is to submit on 29 Oct.
- **You plan; you don't build.**
  - Write every prompt as a file in the brain (`prompts/<date>/<job>.md`) and push it before Sunny pastes it.
  - Say which tool, folder and session each prompt goes in.
- **Check every PR from GitHub before Sunny merges it:**
  - fetch `refs/pull/N/head`, read the diff, recompute every table;
  - probe deterministic filters with adversarial inputs;
  - run the tests merged onto main locally (Node 22 is fine for vitest);
  - put **HOLD #N** first in your reply when something must not merge.
- **Brain vault:** `SunnyBagal/tracehound-brain`. Attach it with `add_repo` (access push).
  - File every pasted agent output verbatim as `prompts/<date>/<job>.out.md`.
  - Update NOW, prs, results, timeline and lessons after each event, then push.
  - The brain is public: no Cal.com comment text, no secrets.
- **Style:** short, blunt, honest. **Answer what Sunny asks.** Don't build unrequested deliverables. On 19 Oct a video nobody asked for wasted his tokens; offer extras in one line instead.

## Read in order
1. `NOW.md`
2. `decisions/P008-reviewbench-preregistration.md` (+ amendment 1)
3. `research/R10-smarter-reviewer.md`
4. `research/R7-cc02-reviewbench.md` (the methodology section)
5. `project/architecture-v2.md` + `decisions/P007-v2-architecture.md`
6. `lessons.md`, top entries

## State (verified on GitHub 20 Oct 17:55)
- **Main `720cc83`** (#48 merged). On 19 Oct: #43, #44, #45, #46, #47, #41 and #48 merged.
- **Open, unchecked; check these first:**
  - **#49 codex-09 learning** (`30a3d44`, 10 commits, 15 files). Prompt: `prompts/2026-10-19/codex-09-learning.md`.
    - Check: the paths are in scope; `learn: 'off'` output is byte-identical; only maintainers' feedback counts (permission check); the adversarial fixtures (null deref vs SQL injection, different extension, read-only 👎, resolved-with-commit, "good catch" un-reject, idempotent).
    - **Codex ends 22 Oct**, so any fix round must happen by then.
  - **#50 rb-01 ReviewBench image** (`1d06203`, 6 commits, 14 files). Prompt: `prompts/2026-10-19/rb-01-reviewbench-image.md`.
    - **It was built from the 18:42 version of the prompt, before Nemotron 3.5 support was added.** `config/prices.json` has no `nvidia/Nemotron-3.5-Super-VL` entry, and there's no 3.5 smoke run.
    - Write an **rb-01b** fix for the same cloud session: accept the 3.5 model id; add a price entry of 0.30/0.90; confirm the reasoning flag works with 3.5 (STOP if it needs a `packages/` change); add a third smoke run on PR 0 with 3.5.
    - Also check: never-fail wrapper, 40-finding cap, contract output schema, no access to `golden/`, image public on ghcr.
  - #42 (ReviewBench spike, draft): close it once #50 merges.
- **Nebius:** console **$30.18** (Sunny, 19 Oct); ledger $25.82 against our cap of $45. **Sunny will top up about $25 before the ReviewBench final run**, then raise the cap to about $75.
- **Claude cloud credit:** ~$85, expires 5 Nov. 2 Ultrareviews left (#2 → integration branch, #3 → final branch).
- **Lanes:**

| Lane | Tool | Where |
|---|---|---|
| Relay | Claude Code | `~/Projects/tracehound-ops` → `~/Projects/TraceHound` |
| Codex | ChatGPT Codex | `~/Projects/TraceHound-codex`, **ends 22 Oct** |
| Claude cloud | claude.ai/code | — |
| Grok | fresh chat each job | — |
| Claude B | second Claude subscription | not set up yet |

Sunny is considering a **Team plan (2 seats)** for Claude Motion; the decision is his.

## What we know (numbers)
- **Blind Cal.com, 10 PRs, run 1:** Augment 73.2 · Cursor 66.7 · **TraceHound 58.3** · Greptile 50.0 · Copilot 46.2 · CodeRabbit 36.7.
  - 3-run mean 54.1 (v1) / 54.2 (x1); Greptile 50.0 / 55.0. So it's statistically tied with Greptile and Cursor, and ahead of CodeRabbit.
- **All 50 PRs:** 4th of 6 (41.2; the dev 5-run-mean row is about 46).
- **Dev 5-run spread of pool-verify-v1:** 35.6–48.0 F (mean 42.3). Run 1 was the lowest.
- **Precision levers all lost on dev:** 058, 064 (vote), 065 (evidence verifier + gate). We stopped tuning precision (P007 update).
- **Contracts (#47):** 10/10 planted breaks caught, 0 false alarms on controls. Not wired into the Action yet.
- **Claims:** "on 10 blind PRs, above Greptile and CodeRabbit, behind Augment and Cursor; the Greptile gap is within noise; 4th of 6 across all 50; every loss published." **Sunny keeps CodeRabbit** in our tests and our numbers, despite §4.2(iv) of its terms. Don't re-argue it.

## Strategy now
1. **ReviewBench is the main lever** (219 PRs, GitHub + Microsoft; grounded F1 is recall-driven). Greptile 27.2, Cursor 17.3, Copilot 40.1.
   - P008 pre-registers 5 candidates: pool-verify-v1, pool-raw and pool-raw-x2 on Nemotron 3 Super, plus pool-verify-v1 and pool-raw on **Nemotron 3.5 Super VL**.
   - Choose by grounded F1 on the 25-PR test runs, then one final run (219 × 3).
   - **Never open `golden/`.**
   - Flow: merge #50 (after rb-01b) → Sunny registers in the portal → maintainer approval (~1 day) → 5 test runs (~$5) → top-up → final (~$22–43).
2. **Nemotron 3.5 Super** is on Token Factory at the same price as 3 Super (SWE-bench Verified 71.1 vs 60.5). It's the biggest "smarter" lever. If it wins the test runs, a 3-run dev check (~$5.4) comes before switching the Action's default.
3. **Seeded cross-file test** on 25 Oct:
   - 30 PRs on the `linea-seeded` / `recall-seeded` mirrors; the answer key is in the private `tracehound-seeds` (`a4a6c83`).
   - Hashes are published in NOW. Tools: TraceHound, CodeRabbit, Greptile, Bugbot.
   - Nobody opens the mirrors until scoring.
4. **Not possible:** fine-tuning or RL (Token Factory post-training has no Nemotron). "Learning" is memory-based: codex-09 now, Bugbot-style learned rules later.
5. Sunny has **Nebius sandbox** access (beta). Candidate use after ReviewBench: "checked fixes" (apply the fix suggestion, run the tests).

## Pending with Sunny
- Start **066** in relay (`prompts/2026-10-19/066-results-docs.md`, cap 0): the R6 wording fixes, the all-50 table with a 5-run-mean row, the levers table, the README headline. Check whether it has run.
- Confirm the **3.5 model id** in the Token Factory console.
- Optional: run **grok-08** in Grok or ChatGPT as a second opinion on R10.
- The X/LinkedIn post, using the Cal.com blind table. He wants a Claude Motion animation, which needs a Team plan.

## Next jobs to write
1. rb-01b (above).
2. Fix round for #49 if needed (must finish by 22 Oct).
3. int-01: wire contracts + learning into the Action (Claude B or cloud).
4. Dev check of 3.5 (after the test runs).
5. Results doc + judges' README, Grok final audit (26 Oct), Devpost, video under 3 min, **submit 29 Oct**.

## Side project
TariffLens starts 30 Oct.
