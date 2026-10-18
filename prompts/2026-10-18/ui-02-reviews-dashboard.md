# ui-02: reviews dashboard (bug cards + fix diff), with no backend

Tool: **Codex GPT Sol 6.1, the UI task.** Folder: `~/Projects/TraceHound-ui` (worktree). Paste this whole file.

---

Goal: P006's dashboard. The site must show **bugs and fixes**, not only a repo graph. It reads TraceHound's real review comments from GitHub's public API in the browser. Graph overlays from the `tracehound-data` branch come in ui-03, after codex-07 merges.

## Step 0 (if any check fails, STOP)
- `git fetch origin main:main` (no plus), then `git switch -c ui/reviews-dashboard main`.
- `git status --porcelain` prints nothing.
- `packages/action/src/action.ts` on main contains `tracehound:v1` (the review data footer).

## Build (small commits, each with tests, typecheck and viewer build green)
1. **Data layer** in `viewer/lib/reviews.ts`. Unauthenticated GitHub REST calls from the browser:
   - list PRs (open plus the last 30 closed);
   - for each PR, read its reviews, keep those from `github-actions[bot]` whose body contains `<!-- tracehound:v1 {json} -->`, and parse that JSON;
   - read the review comments (path, line, body, any suggestion block).
   Cache in memory per page. On a rate limit (60 req/h), show a quiet "GitHub rate limit, try later" state and never crash. Tests use fetch fixtures.
2. **Route `/reviews?repo=owner/name`:** a PR list with title, state, and the count of findings and suggestions, read from the footer.
3. **Route `/reviews/pr?repo=…&n=…`:** one card per finding, showing:
   - the file:line with a GitHub link;
   - the comment text;
   - a **fix diff**: the original vs the suggestion block, using the existing code renderer;
   - a **"Commit fix on GitHub"** button linking to the PR's Files tab at that comment;
   - the source (diff/ctx/both, from the footer).
   Label everything "model-generated, not tested".
4. **Benchmark page `/benchmark`:** render the tables from `docs/bench/results.md`'s numbers. Copy them as static data with a source line; do not invent any. Include the "all 50 PRs" view only if `docs/bench/results.md` contains it, otherwise skip it.
5. **Landing:** add a "Reviews" entry next to the graph. Make `SunnyBagal/tracehound-demo` the default repo.

## Checks
- Screenshots of the PR list and of demo PR #3's bug card with its fix diff, saved to `docs/ui/round-2/`.
- Nebius spend is $0. Add no dependencies. Touch only `viewer/` and `docs/ui/`.

## PR
Open it to main as ready for review, titled `viewer: reviews dashboard with bug cards and fix diffs`. Never merge.

STOP AND REPORT: the PR number, head sha, commits, test count, screenshots, and anything skipped.
