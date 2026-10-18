# codex-07: the Action writes graph data to a `tracehound-data` branch

Tool: **ChatGPT Codex, new task.** Folder: `~/Projects/TraceHound-codex`. Paste this whole file.

---

Goal: backlog item 2. Every repo that installs the TraceHound Action gets a persistent graph history with no server. The viewer will read it from raw.githubusercontent.com in a later UI job (ui-02/03).

## Step 0 (if any check fails, STOP)
- `git fetch origin main:main` (no plus), then `git switch -c codex/data-branch main`.
- main contains the merge of **#40** (`gh pr view 40 --json state` shows MERGED).
- `git status --porcelain` prints nothing.

## 1. Re-pin dogfood (its own commit)
In `.github/workflows/tracehound-review.yml`, change the pin from `50e1d4a…` to the full sha of #40's merge commit on main, so TraceHound's own PRs get fix suggestions. That is the only change in this commit.

## 2. Data writer (offline-testable, no model calls)
Add `packages/action/src/data.ts`.
- **On `push` to the default branch:**
  - run the analyzer CLI (`packages/analyzer/src/cli.ts --repo <workspace> --naming heuristic --out <tmp>`) using the repo's `tracehound.json` if one exists;
  - write the snapshot to `snapshots/<sha>/<analyzerVersion>.json` and update `snapshots/index.json` (the same manifest shape the viewer already reads).
- **On `pull_request`:**
  - run `tracehound changes --repo <workspace> --diff <base>..<head> --json`;
  - write `changesets/pr-<number>-<headSha7>.json`;
  - update `changesets/index.json` with `{ id, repo, pr, title, headSha, baseSha, state: "open", reviewFooter? }`. `reviewFooter` is the parsed `tracehound:v1` JSON if the review step produced one.
- **On `pull_request` closed:** set `state` to `"merged"` or `"closed"` in the index. Never delete files.
- **Commit and push:**
  - commit to an **orphan branch `tracehound-data`**, creating it if it's missing, using the workflow token;
  - the commit message is `data: <event> <sha7>`;
  - retry once on a non-fast-forward push (fetch, then reapply).
- **Failures:** the analyzer is TypeScript-only. If it fails or the repo has no TS, write nothing, log one line, and **never fail the review job**.
- **Tests:** offline with fakes for git and the analyzer:
  - orphan-branch creation;
  - append vs update of the index;
  - closed and merged state;
  - the retry path;
  - "analyzer failed → nothing written, exit 0".

## 3. Action wiring
- New input `data-branch: on|off`, default `off`. It has to be opt-in because it needs `contents: write`.
- Add a `push` + `pull_request` (including `closed`) example in `docs/action/example-workflow.yml`, plus a README section on permissions.
- In the composite action, the data step runs after the review step, and only when `data-branch` is `on`.

## 4. Live check (demo repo only; costs nothing extra, the analyzer has no model calls)
- In `SunnyBagal/tracehound-demo`:
  - pin the workflow to this branch's head sha, with `data-branch: on` and `contents: write`;
  - push one no-op commit to main;
  - close and reopen demo PR #3.
- Record:
  - that `tracehound-data` exists;
  - the files written and the index entries;
  - the run ids.
- Afterwards, re-pin the demo workflow to the sha it had before this job, and leave `tracehound-data` in place.

## 5. PR
- Use small commits, each green.
- Open the PR to main as ready for review, titled `action: write graph snapshots and change sets to tracehound-data`.
- Put the live check results in the description. Never merge.

Rules:
- Touch only:
  - `packages/action/`;
  - `.github/actions/tracehound-review/`;
  - `.github/workflows/tracehound-review.yml` (step 1 only);
  - `docs/action/`.
- Do not change `packages/analyzer/` or `scripts/bench/`. Relay job 064 is working there.

STOP AND REPORT: the PR number, head sha, commits, test count, the live check table, and anything skipped.
