# bunny-04: Devpost drafts, everything that doesn't need the final numbers

Tool: **Bunny**, a **new `claudeb` session** in `~/Projects/TraceHound-claude2`. Paste everything below the line. No `.env`; make no Nebius calls.

---

TraceHound goes to Devpost on **29 Oct** (deadline 30 Oct 22:30 IST). The final numbers land on 26 Oct: the ReviewBench final, the seeded test, and the v2 Cal.com run (decision 069). This job writes every submission text that doesn't depend on them, with `[[PLACEHOLDER]]`s where numbers go, so 26–27 Oct is a fill-in job.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin --tags`; `git status --porcelain` is empty. Branch `docs/devpost-drafts` from `origin/main`.
- In a plain shell, the `GIT_*` env check prints nothing; `pnpm test` and `pnpm typecheck` are green.
- Read:
  - `README.md`, and your own draft README on PR #54 (`git show origin/pull/54/head:README.md` if fetched, or the branch);
  - `docs/bench/results.md`, `docs/bench/dev-3.5.md`, `docs/action/README.md`;
  - `docs/decisions.md` (skim headers 001–068; read 050, 059, 061–068 in full);
  - `FEEDBACK.md` (all);
  - `CREDITS.md`, `LICENSE`, `package.json`;
  - `git log --reverse --format='%h %ad %s' --date=short | head -40` and `git log --format='%h %ad %s' --date=short | wc -l`.
- **Don't open** `linea-seeded`, `recall-seeded`, `tracehound-seeds`, ReviewBench's `golden/`, or anything Cal.com under `~/Projects/tracehound-bench/test/`.

## Facts to use (from the planning chat, 23 Oct)
- **Product:** a GitHub Action that reviews PRs with NVIDIA Nemotron via Nebius Token Factory. Pipeline `pool-verify-v1`: two generators (diff-only and repo-context), pooled, then a context-aware verifier.
- **Model:** since decision 068, every review stage runs on **`nvidia/Nemotron-3.5-Super-VL`** (served from Token Factory us-north1). Suggested fixes stay on `nvidia/nemotron-3-super-120b-a12b` (us-central1). Failed chunks give partial reviews.
- **Also in the Action (#53):** cross-file contracts, which flag exports/routes/env/events a PR changes that break files it didn't touch. Plus learning from maintainer-rejected comments (off by default).
- **Freeze tag:** `review-freeze-2` → `0aeef06`.
- **Dashboard:** https://tracehound-tau.vercel.app (graph, `/reviews`, `/benchmark`).
- **Public claim wording (decision 066), verbatim where a claim is made:** "On 10 blind PRs, statistically tied with Greptile and Cursor; run-1 F 58.3 vs CodeRabbit 36.7, but not a significant lead (the 95% interval includes 0; ahead in 96% of resamples). 4th of 6 across all 50 under both judges. Every loss is published."
  - **Never** "beats", never "3rd of 6" as a ranking.
  - 3.5 on dev: say "2 dev runs", and that the x1 interval includes 0.
- **Devpost rules (R14):**
  - video under 3 min, public on YouTube, shows it working;
  - **no third-party trademarks in the video**: no competitor names or logos (in the Devpost text too: say "compared with five commercial reviewers; see the results doc");
  - the demo must work **without a judge's own Nebius key**;
  - MIT license visible in the repo About;
  - English;
  - feedback on Nebius and NVIDIA;
  - an existing project must be "significantly updated" after **26 Aug**, with an explanation;
  - judging is pass/fail, then 4 equal criteria: Technological Implementation (incl. how well NVIDIA models are used), Design, Potential Impact, Quality of the Idea.
- **Known demo caveat:** the reviews on the demo repo's PRs #1–#5 date from 17–18 Oct (Nano generators, before freeze-2). The 26 Oct seeded test produces fresh reviews on the frozen config. Don't present the old demo stats ("2 of 4 seeded lines") as the current config.

## What to write (one commit per file; docs only; run the gates as their own commands before each commit)
All in a new folder `docs/devpost/`:
1. **`writeup.md`**: the Devpost fields: tagline (≤ 60 chars), inspiration, what it does, how we built it, challenges, accomplishments, what we learned, what's next, built with.
   - Lead with technical implementation and Nemotron use (the first judging criterion).
   - Numbers go in as `[[RB-FINAL]]`, `[[SEEDED]]`, `[[CALCOM-V2]]`, `[[ALL50-V2]]`, with a one-line note on each of what fills it.
   - Honest about losses: the graph didn't help the repair agent (050), and votes and evidence gates lost (064, 065). "Every loss is published" is a feature, so show it.
2. **`what-changed-since-26-aug.md`**: what existed before 26 Aug vs what was built after, from git history and the decisions log.
   - The repo's first commit is 30 Sep, and decisions 001–031 appear that day.
   - Anything you can't establish from the repo goes in as `[[SUNNY: …]]` (e.g. what FixFlow was before 26 Aug). Don't guess.
3. **`nebius-feedback-summary.md`**: a one-page summary of `FEEDBACK.md`: what was called, counts, and the concrete problems with exact error strings. Include the us-central1 listing of 3.5 that answers 404, and 3.5's invalid-JSON chunks. Then what would have helped. Facts first; opinions labelled.
4. **`video-script.md`**: a ≤ 2:50 script, shot by shot, with a time for each. No competitor names or logos. Show:
   - a live PR review on the dashboard and GitHub;
   - the pipeline diagram;
   - the Nemotron + Token Factory usage;
   - TraceHound's own numbers (placeholders);
   - "every loss is published";
   - how to install the Action (one YAML block).
5. **`checklist.md`**: R14's table as a checklist with the current status of each item and who does it. Include: the license shows in GitHub's About sidebar (check with `gh repo view --json licenseInfo`, or say you couldn't), the README pin, FEEDBACK, the video, the demo without a key.

## Rules
- Docs only. **Don't touch:** `README.md` (the 26 Oct fill on #54), `docs/bench/results.md`, `viewer/`, `packages/`, `scripts/`, `config/`, `.github/`, `AGENTS.md`.
- No invented numbers, quotes or dates. Placeholders instead.
- Push. Open a **draft** PR titled `docs: Devpost drafts (bunny-04)`. Never merge.

## STOP AND REPORT
Final message:
- PR number and head sha;
- one line per commit;
- test counts;
- every `[[SUNNY: …]]` question you left;
- anything you couldn't do.

Also write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-23-bunny-04.md` (rules in `inbox/README.md`).
