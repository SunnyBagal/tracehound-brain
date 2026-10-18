# codex-06c: make suggestions committable (follow-up to merged #38)

Tool: ChatGPT Codex. Use the **same task that made #38** if it's still open, otherwise start a new task. Folder: `~/Projects/TraceHound-codex`. Paste this whole file.

---

PR #38 is already merged into main (merge commit `41a92a8`). Do not add commits to `codex/fix-suggestions`.

Why this follow-up: live check 2 found that demo #2's suggestion has no "Commit suggestion" button.
- The suggestion spans RIGHT lines 59–61. Line 60 is the added line.
- Lines 59 and 61 are context lines, and the PR's deleted line sits between 59 and 60 in the diff.
- GitHub won't apply a multi-line suggestion whose range crosses a deleted line.

The seeded demo PRs are what we show in the video, so every posted suggestion must be committable.

## Step 0 (if any check fails, STOP)
- `git fetch origin main:main` (no plus), then `git switch -c codex/fix-committable main`.
- `git log --oneline -1 main` is `6058d53` (the merge of #39) or later.
- `git status --porcelain` prints nothing.

## 1. Guard (one commit, with offline tests)
- In `validateFix`, reject a range from `startLine` to `endLine` if, within its hunk, any `-` line falls between the first and last line of the range in diff order.
- A single-line range is always fine.
- Tests:
  - demo #2's shape (context, deleted, added, context; range 59–61) is rejected;
  - the same fix narrowed to line 60 only is accepted;
  - a range over consecutive added lines is accepted.

## 2. Prompt hint (its own commit)
In `packages/action/src/fix-prompt.ts`, add one line: "Prefer the smallest range; if the defect is on added lines, replace only those added lines." This is the product-only prompt; the benchmark prompts stay untouched.

## 3. Live re-check: demo #2 only (at most 2 runs)
- Pin the `tracehound-demo` workflow to this branch's head sha.
- Close and reopen demo #2. Record:
  - the run id;
  - comments and suggestion blocks;
  - whether the "Commit suggestion" button shows (signed in);
  - the footer's fixes A/V/R.
- If the fix is now rejected (V = 0), that's acceptable: the finding still posts. Report it. Do not loosen the guard.
- Don't click commit. Leave the demo heads unchanged.
- After the run, pin the demo workflow back to a sha that is on TraceHound main: use `41a92a8` until this PR merges.

## 4. PR
- Open a PR to main as **ready for review**, not a draft, titled `action: keep fix suggestions committable`.
- Put the live row in its description.
- Do not touch `.github/workflows/tracehound-review.yml` in TraceHound; the dogfood re-pin is a separate step after merge.
- Do not merge.

STOP AND REPORT: the PR number, head sha, the commits, the test count, the live row, and anything skipped.
