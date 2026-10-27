# seed-01a: an agent runs seed-01 (open the seeded PRs, wait for the reviews, score), Sunny does only the key

Replaces seed-01 part A's hand steps A1, A3, A4 and part B's tool choice. The pre-registration (key, SCORING.md, their hashes) is unchanged.

**Sunny, by hand first (≈1 min; the key never goes through an agent).** In a plain terminal:
```
cd ~/Projects/TraceHound
for r in linea-seeded recall-seeded; do grep -E '^NEBIUS_API_KEY=' .env | cut -d= -f2- | tr -d '"' | gh secret set NEBIUS_API_KEY -R SunnyBagal/$r; done
gh secret list -R SunnyBagal/linea-seeded; gh secret list -R SunnyBagal/recall-seeded
```
Both lists must show `NEBIUS_API_KEY`. Nothing is printed except those lists.

**Then the agent.** Tool: **Claude Code, account A** (relay is idle; this is a plain session, not the relay loop). New folder:
```
cd ~/Projects && git clone https://github.com/SunnyBagal/tracehound-seeds && cd tracehound-seeds && claude
```
Paste everything below the line. It runs about 1–2 hours, mostly waiting; let it run.

Decisions (chat, 27 Oct 22:45): **Bugbot skipped** (fewer moving parts, quota; disclosed). CodeRabbit and Greptile not run (already disclosed in seed-01). Cost: TraceHound on 3.5, about $0.07–0.10 per PR with VAT, about $3 for 30 PRs, from the Nebius balance.

---

You run a **pre-registered seeded test** of TraceHound, an AI PR reviewer. The seeds, the key and the scorer were written on 19 Oct in this repo (`a4a6c83`), and their hashes were published before any PR opened. You open the seeded PRs on two mirror repos, wait for TraceHound's GitHub Action to review each one, then score the reviews with the committed scorer. **Don't change the scorer, the key, SCORING.md or the seed branches.**

## Rules
- Repos you may touch: `SunnyBagal/linea-seeded`, `SunnyBagal/recall-seeded` (one workflow file on the default branch, and opening PRs), and this repo (`raw/`, `RESULTS.md`, a disclosed local copy of the open-PR script). Nothing else; **never `SunnyBagal/TraceHound`**.
- **Never print, read or echo `NEBIUS_API_KEY`** or any secret. You don't need it: the mirrors already have it as a repository secret.
- Don't comment on, close, merge or edit any seeded PR. Don't trigger CodeRabbit, Greptile or Bugbot.
- **No model calls** except the ones TraceHound's Action makes on the mirrors. If the scorer itself would call a model, STOP and report first.
- More than 40 PRs about to open in total, or any PR opening outside the seed list: STOP.

## Step 0 (STOP and report on any failure)
1. `git log --format=%H | grep -q ^a4a6c83` (the pre-registration commit is in history); `sha256sum key.json SCORING.md` (or `shasum -a 256`) gives exactly `abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1` and `ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d`.
2. Read this repo's README/runbook, SCORING.md, `open-prs.sh` and the scorer (with its tests). Run the scorer's offline checks (13 expected).
3. `gh auth status` works; you can push to both mirrors (`gh repo view SunnyBagal/<mirror> --json viewerPermission` is ADMIN or WRITE). `gh secret list -R SunnyBagal/<mirror>` shows `NEBIUS_API_KEY` on both.
4. On each mirror: the default branch name, that no TraceHound workflow exists yet, the seed branches that exist (count per mirror, against the seed list), and that **no PR is open yet**.

## Step 1: the workflow on both mirrors
On each mirror's default branch, add `.github/workflows/tracehound-review.yml` with **exactly** this content (one commit per mirror, message `ci: TraceHound review (review-freeze-2, 3.5) for the seeded test`):

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

The seed branches don't need rebasing: a `pull_request` run uses the workflow from the PR's merge with the base. If a mirror's Actions are disabled, enable them (`gh api -X PUT repos/SunnyBagal/<mirror>/actions/permissions -F enabled=true -f allowed_actions=all`) and record it.

## Step 2: open the PRs
- Use `open-prs.sh` as committed if it can skip the CodeRabbit trigger comment and set the wait between PRs; otherwise copy it to `runs/open-prs-seed01a.sh`, change **only** (a) no CodeRabbit trigger comment and (b) 90 s between PRs, and commit that copy with a header listing both changes (a disclosed deviation). Nothing else differs: same branches, titles, bodies, order.
- Open **one PR first**, wait until its TraceHound run finishes, and check: the run is green, a review by `github-actions[bot]` exists, and its header names `Nemotron-3.5-Super-VL` (lesson 20 Oct: the live config must match the benchmark). If not: STOP and report.
- Then open the rest.

## Step 3: wait for every review
Poll every 5 minutes (`gh run list -R SunnyBagal/<mirror> --workflow tracehound-review.yml --limit 50 --json status,conclusion,headBranch,databaseId`) until every PR's run has completed, up to 3 hours. A failed run: rerun it **once** (`gh run rerun <id>`); record both. Don't rerun a green run that posted no findings (that's a result).

## Step 4: score (seed-01 part B)
1. Collect every TraceHound review and review comment on every seeded PR (author `github-actions[bot]`, TraceHound header) exactly as SCORING.md says; save the raw JSON under `raw/`.
2. Run the scorer. Report overall and per mirror: seeds caught / total by category (typed, keyed, semantic, in-diff), false alarms on the controls, comments per PR, and every PR where TraceHound posted nothing, posted a partial review ("N of M reviewer chunks failed"), or whose run failed (with the run status).
3. Deviations, plainly: CodeRabbit, Greptile and Bugbot not run; TraceHound at `review-freeze-2` on 3.5 (not 3 Super, which the 19 Oct plan assumed); the open-PR script changes (if any); anything SCORING.md expected that didn't happen.
4. `RESULTS.md` in this repo with the tables and PR links (seed descriptions are fine here: this kit becomes public after scoring). Commit, push.

## Report (final message)
Step 0 results; per mirror: PRs opened, runs green / red / rerun, partial reviews; the score tables; deviations; links to **3 good TraceHound catches and 1 miss** (for the video); the Nebius spend if the run logs show it.

Also write it to `~/Projects/tracehound-brain/inbox/2026-10-28-sunny-seed-01.md` (rules in that repo's `inbox/README.md`: only that file; `git pull --rebase`, commit, push). The brain is public: **counts and PR links only, no seed descriptions or code** in that file.
