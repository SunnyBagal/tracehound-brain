# codex-16: README first screen completed

PR #54: https://github.com/SunnyBagal/TraceHound/pull/54
Head SHA: `d762b1478903007e1ed8e7002c18c3c54b1efb12`.

The prior merge was aborted; the worktree was clean at `2bb3144`. The retry had
only README.md conflicted, so the branch version was kept and the merge committed
as `652bf8e45a4d662d3a37abddda94fc05b603214b`. Before editing, README.md was
byte-identical to `2bb3144` and contained no conflict markers.

The single authored edit commit is `d762b14`: README.md only, 16 insertions and
7 deletions. It adds the six-line no-key block before Try it; puts the key,
same-repository restriction, cost and partial-review caveat before the YAML;
moves the fork warning up; and explains pnpm demo's heuristic fallback,
snapshot overwrite and `git checkout -- snapshots/` restoration.

Checks:

- `grep -c "TBD-SEEDED" README.md`: 2; both literal placeholders preserved.
- Prohibited wording added versus the original origin/docs/judges-readme
  (`2bb3144`): 0 matches for beats, best, outperforms, or state of the art.
- Reused the judge's links.py, adapted to this merged worktree and edited README:
  25 relative-link occurrences, 21 unique targets, 0 broken; all 8 anchor
  occurrences pass. External URLs were not fetched.
- No-key block: 6 lines, meeting the limit.
- Results, Losses and What's next: byte-identical to `2bb3144`.
- Action YAML: byte-identical; no conflict markers; `git diff --check` passed.
- Branch pushed; local and remote heads match; product worktree clean.
- PR #54 reports `isDraft: false`; no readiness API or command was invoked.
- Tests/typecheck: not run (documentation-only edit; requested documentation
  checks passed). Model calls: 0; model spend: $0.

No requested README work remains. The brain inbox update replaces the prior STOP
report at the requested path; the earlier report had been archived by another
brain commit before this update.

Diff affecting the first 60 lines of README.md, versus `2bb3144`:

```diff
diff --git a/README.md b/README.md
index 9f89212..c30b18f 100644
--- a/README.md
+++ b/README.md
@@ -4,0 +5,7 @@ An open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on
+## See it without a key
+
+- [https://tracehound-tau.vercel.app](https://tracehound-tau.vercel.app) — reviews as bug cards, the benchmark page, and the component graph.
+- [Demo repo](https://github.com/SunnyBagal/tracehound-demo).
+- On 10 blind Cal.com PRs, TraceHound is statistically tied with four of five commercial reviewers and ahead of one (CodeRabbit; only 10 PRs). [Results](docs/bench/results.md).
+- Locally, `pnpm install`, `pnpm fixture`, and `pnpm test` need no key; see [Quick start](#quick-start).
+
@@ -7,2 +14,5 @@ An open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on
-Add a repository secret `NEBIUS_API_KEY` (your Token Factory key), then save this as
-`.github/workflows/tracehound-review.yml`:
+Running the Action needs a Nebius Token Factory key as the repository secret `NEBIUS_API_KEY`.
+It runs on same-repository pull requests only; pull requests from forks are skipped because forks
+don't get repository secrets. Reviews cost about $0.06–0.07 per pull request on 3.5, pre-VAT.
+When a chunk returns invalid JSON (about 7 of 40 dev pull requests per run), the review is partial
+and says so. Save this as `.github/workflows/tracehound-review.yml`:
@@ -45,2 +55 @@ This is the review job of [the example workflow](docs/action/example-workflow.ym
-graph-history job. Pull requests from forks are skipped, because forks don't get repository
-secrets. Inputs, permissions and limits are in [docs/action/README.md](docs/action/README.md).
+graph-history job. Inputs, permissions and limits are in [docs/action/README.md](docs/action/README.md).
```
