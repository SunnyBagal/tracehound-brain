# ur-01: fix the Ultrareview findings on #43 (same Claude cloud session that ran the Ultrareview)

Tool: **the Claude cloud session that produced the 10 findings** on `codex/data-branch`. Reply in that session with everything below the line. It uses the cloud credit.

---

Yes, go ahead. The planning chat checked findings #1, #2, #4 and #8 against the code and confirmed them:
- `--diff ${pr.base.sha}..${sha}`;
- `{ ...previous, ...entry }` overwriting the state;
- an unconditional `upsertManifest`;
- `{ ...env }` in `createDataDependencies`;
- `GIT_CONFIG_GLOBAL=/dev/null` on the workspace git calls.

Work on branch `codex/data-branch` (PR #43). Make one small commit per group, each with a test that fails before the fix, and keep `pnpm test` and `pnpm typecheck` green. Stage by explicit path. Never force-push, never merge.

1. **History correctness: #1, #2 and #3 (one commit each).**
   - #1: compute `git merge-base <base> <head>` in the workspace (fetch the base if needed, with a depth that reaches the merge base). Diff from that base and record it as `baseSha` in the entry.
   - #2: snapshots only move `latest` forward, meaning the event's sha is the default-branch tip or a descendant of the recorded latest. Otherwise write the snapshot file but leave the index alone. Change sets never downgrade: an `open` event can't overwrite `merged` or `closed`, and `merged` beats `closed`.
   - #3: also retry once on `[remote rejected]` with `cannot lock ref` / `failed to update ref`. Keep the "never retry auth failures" rule.
2. **Environment: #8 and #4 (one commit).**
   - Build both child envs from `cleanGitEnv()`.
   - Apply the auth extraheader, `GIT_CONFIG_GLOBAL=/dev/null` and `NOSYSTEM` **only** to the data-branch clone's git calls, never to the workspace calls. That keeps `safe.directory` working, and the token stays out of the workspace git.
3. **Footer: #9 and #10 (one commit).** Write the review footer temp file as plain JSON and read it with `JSON.parse`. If you still parse the HTML comment anywhere, match the **last** `tracehound:v1` comment, anchored to the end of the body.
4. **Docs honesty: #6 and #7 (one commit, docs only).**
   - In `docs/action/README.md`, say the data step shares the job timeout: if the job times out after the review posts, the data write is lost.
   - In `docs/action/example-workflow.yml`, split concurrency: reviews `cancel-in-progress: true` per PR, data writes serialized per repo with `cancel-in-progress: false`, either as separate jobs or as two workflows. Keep the permissions minimal: `contents: write` only on the data job.
5. **#5 (full checkout of the data branch) goes to the backlog.** Add one line to `docs/action/README.md` under "Limits".

Then:
- Run the demo live check again for **one** no-op push and **one** PR reopen. Pin the `tracehound-demo` workflow to the new head sha, then restore the old pin afterwards.
- Update `docs/action/data-branch-live-check.md`.
- Post a PR comment listing each finding as fixed, deferred or rejected, with commit shas.

Rules:
- Touch only `packages/action/`, `.github/actions/tracehound-review/`, `docs/action/`.
- Make no Nebius calls beyond what the Action itself makes in the live check.

End with: the head sha, the commits, the test count, and the live check rows.
