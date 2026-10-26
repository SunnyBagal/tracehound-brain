# seed-01: seeded PRs on the current config (TraceHound, plus Bugbot if you want it)

Two parts: **A** is by hand (Sunny, about 30–45 min of clicks, then waiting), **B** is a scoring session after the reviews are in.

What changed from the 19 Oct pre-registration (disclose in results): CodeRabbit and Greptile are **dropped** (not installed, Greptile consent never sent). TraceHound runs at `review-freeze-2` with the explicit 3.5 model. Bugbot is optional. The seed key, its sha256 and SCORING.md are unchanged (`sha256(key.json) = abbde009…391bc1`, `sha256(SCORING.md) = ebac16b3…a5c1d`, published 19 Oct 17:50 IST).

Cost: TraceHound about $0.07–0.10 per PR with VAT → about $3 for 30 PRs, from the $43 balance. Bugbot: X quota.

## A. Sunny, by hand

**A1. Add the TraceHound workflow to both mirrors** (`SunnyBagal/linea-seeded`, `SunnyBagal/recall-seeded`). On each, on the default branch, create `.github/workflows/tracehound-review.yml` with exactly this (it's the demo repo's file):

```yaml
name: TraceHound review

on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

permissions:
  pull-requests: write
  contents: read

concurrency:
  group: tracehound-${{ github.event.pull_request.number }}
  cancel-in-progress: true

jobs:
  review:
    if: github.event.pull_request.head.repo.full_name == github.repository && !github.event.pull_request.draft
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      - uses: actions/checkout@v4
        with:
          ref: ${{ github.event.pull_request.head.sha }}
          persist-credentials: false
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2
        with:
          nebius-api-key: ${{ secrets.NEBIUS_API_KEY }}
          model: 'nvidia/Nemotron-3.5-Super-VL'
          max-comments: '10'
          dry-run: 'false'
```

Commit it straight to the default branch **before** any seeded PR opens (the seed branches must be based on a commit that has it, or the workflow won't run on them: check the mirror's README/runbook in `tracehound-seeds` for how branches are created; if they're already pushed from an older base, the workflow still runs because `pull_request` uses the base branch's workflow file).

**A2. Secret:** on each mirror, Settings → Secrets and variables → Actions → New repository secret → `NEBIUS_API_KEY` = the same key the demo repo uses.

**A3. Bugbot (optional, recommended):** cursor.com/automations/from-cursor/bugbot → SunnyBagal row → enable `linea-seeded` and `recall-seeded`. Set **Trigger Mode → Automatic** for the duration (back to Manual Only after), Autofix Off, PR Summaries Off. Bugbot results are README-only (no names in Devpost/video, R14), and Cursor's terms need the replication kit published (R13: make `tracehound-seeds` and the mirrors public after scoring).

**A4. Open the PRs.** Follow `tracehound-seeds`' instructions for `open-prs.sh`. It was built to post a CodeRabbit trigger and wait 450 s between PRs (3 h 40 m for 30). For this run:
- remove or skip the CodeRabbit trigger comment;
- shorten the wait to **90 s** if the script has a setting for it (TraceHound's job takes about 2–4 min, and the concurrency group is per PR, so overlap is fine). If it has no setting, run it as is and let it go.
- **Don't open, read or comment on any seeded PR yourself** until B starts, and don't paste their content to any chat.

**A5. Tell the chat** when the last PR has opened, with: number of PRs opened per mirror, and the Actions tab's count of green / red TraceHound runs on each mirror.

## B. Scoring session (after every PR has a TraceHound run finished)

Tool: **Bunny** (account B), after bunny-06 is done, in a new folder: `cd ~/Projects && git clone https://github.com/SunnyBagal/tracehound-seeds && cd tracehound-seeds && claudeb`. Paste everything below the line.

---

You are scoring a pre-registered seeded test. The seeds, the key and the scorer were written on 19 Oct in this repo (`a4a6c83`) and their hashes published before any PR opened. **Don't change the scorer, the key or SCORING.md.** No model calls except any the scorer itself makes as SCORING.md specifies (if it makes any, STOP and report first: chat must approve the spend).

Step 0 (STOP and report on any failure):
- `git log -1 --format=%H` shows the commit containing `a4a6c83` in its history; `sha256sum key.json SCORING.md` match `abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1` and `ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d`.
- Read SCORING.md fully, and the scorer's own README/tests; run its offline checks (13 expected).
- `gh` is authenticated and can read `SunnyBagal/linea-seeded` and `SunnyBagal/recall-seeded`.

Steps:
1. Collect every review comment on every seeded PR, for each tool present (TraceHound: author `github-actions[bot]` with the TraceHound header; Bugbot: `cursor[bot]` if enabled), exactly as SCORING.md says (raw JSON saved under `raw/` in this repo).
2. Run the scorer for each tool. Report, per tool and overall: seeds caught / total by category (typed, keyed, semantic, in-diff), false alarms on the controls, comments per PR, and every PR where the tool posted nothing or its run failed (with the Actions run status).
3. Record deviations from the pre-registration plainly: CodeRabbit and Greptile not run; TraceHound at `review-freeze-2` on 3.5 (not 3 Super); Bugbot included or not; anything SCORING.md expected that didn't happen.
4. Write `RESULTS.md` in this repo with the tables (counts and PR links; seed descriptions are fine here because the kit becomes public), commit, push.
5. Write the summary (tables, links to 3 good TraceHound catches and 1 miss for the video, deviations) to `~/Projects/tracehound-brain/inbox/2026-10-2x-sunny-seed-01.md`, then `git pull --rebase`, commit, push.

Rules: don't touch TraceHound's repo; don't comment on, close or merge any seeded PR; never print the Nebius key.
