# codex-09: learn from feedback (S8): suppress what maintainers rejected; read convention files

Tool: **ChatGPT Codex**. Folder: `~/Projects/TraceHound-codex`. Start a **new task** and paste everything below the line. **This must land by 22 Oct**, when the Codex subscription ends.

---

You're adding TraceHound's learning stage. When a maintainer rejects a TraceHound comment, similar comments stop being posted on that repo. Convention files the repo already keeps for other AI tools are read as well. Everything is deterministic: no model calls, no embeddings and no network beyond the GitHub API the Action already uses.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` prints nothing. Create branch `codex/learn-v1` from `origin/main`, which must contain `b98f753` (the #45 merge).
- `pnpm test` and `pnpm typecheck` are green before you change anything.
- Read `packages/analyzer/src/gate/gate.ts` (the `Suppression` interface), `packages/action/src/action.ts`, `packages/action/src/data.ts` and `docs/action/README.md`.
- **Don't open** `SunnyBagal/linea-seeded`, `SunnyBagal/recall-seeded` or `SunnyBagal/tracehound-seeds`.

## What to build
Make one commit per item, each with tests, and keep `pnpm test` and `pnpm typecheck` green. Stage files by explicit path.

1. **`packages/analyzer/src/learn/fingerprint.ts`**
   - `fingerprint(finding)` takes a finding and returns `{ path, ext, tokens }`.
   - Building `tokens`: lowercase the body; drop code spans, numbers, punctuation and a fixed stop-word list; keep the remaining word set.
   - `similar(a, b)` is true when the token-set Jaccard similarity is at least 0.5 and the two share a path, or share an extension and the Jaccard is at least 0.7. Put both thresholds in named constants.

2. **`packages/analyzer/src/learn/suppress.ts`**
   - `FeedbackSuppression` implements the gate's `Suppression` interface. It is built from a list of rejected fingerprints, each carrying `{ repo, pr, commentId, reason }`.
   - `matches()` returns true for a finding similar to any rejected fingerprint. The match has to be explainable, so `explain(finding)` returns the source PR, comment and reason.

3. **`packages/action/src/learn.ts`: collect feedback** for the current repo from TraceHound's own past comments, meaning review comments and review bodies written by `github-actions[bot]` that carry the `tracehound:v1` footer or sit in such a review.
   - Look at the last 30 PRs updated in the past 60 days.
   - A comment counts as **rejected** if any of these holds:
     - (a) it has a 👎 (`-1`) reaction;
     - (b) its thread was resolved (GraphQL `reviewThreads.isResolved`) and no later commit in the PR touched that file within ±5 lines of the comment;
     - (c) a reply says one of: "false positive", "not a bug", "won't fix", "wontfix", "nit", "ignore", "not an issue", "by design" (case-insensitive, whole phrase).
   - **Only users with write, maintain or admin permission count.** Check them with `GET /repos/{o}/{r}/collaborators/{user}/permission`, cached per run. On a public repo, anyone could 👎, so this rule stops strangers from silencing the reviewer.
   - Each rejected comment carries its reason (`reaction` | `resolved-unchanged` | `reply:<phrase>`) and its URL.
   - A 👍 or a reply like "fixed" or "good catch" **un-rejects** that same comment. The latest signal wins.

4. **Storage on `tracehound-data`:** add a `feedback` update kind to `data.ts`, writing `feedback/rejected.json`, the full list, rewritten each time (deterministic order, idempotent).
   - Collect and write it during the data job, which already holds `contents: write`.
   - Reuse the existing retry and push rules, and keep #43's rules: `cleanGitEnv`, and the token only on the branch git.

5. **Using it in the review (behind a new input, `learn`, default `'off'`):**
   - When `learn: 'on'`, the review step reads `feedback/rejected.json` from `tracehound-data` with a read-only fetch; if the branch or file is missing, that's an empty list. It then drops matching findings **after** the pool-verify-v1 pipeline and before posting.
   - Each drop goes into the footer record as `suppressed: [{ path, line, matchedComment, reason }]`.
   - The review body gets one line: `N findings suppressed by maintainer feedback (details in footer)`.
   - With `learn: 'off'`, the posted output is byte-identical to today's. Add a test that proves it.
   - **Change nothing in `packages/analyzer/src/review/`**, nor in pool-verify-v1's prompts or code.

6. **`packages/analyzer/src/learn/conventions.ts`: read only, not wired into any prompt.**
   - It reads these files from the head checkout when present: `AGENTS.md`, `CLAUDE.md`, `.cursor/BUGBOT.md`, `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md` (keep those whose `applyTo` glob matches a changed file) and `.tracehound/rules.md`.
   - It returns `{ files: [{ path, chars }], text }`, capped at 4,000 characters per file and 8,000 in total, with each cut noted in `text`.
   - Add a CLI `tracehound conventions --repo <dir> --changed <file-list> --out <file.json>` (one line in `bin.ts`).
   - A later job decides whether this text reaches a prompt, because doing so changes the benchmarked pipeline.

7. **Adversarial fixtures, required** (add them to `packages/analyzer/test/learn.test.ts`):
   - A rejected "null dereference of `user` in `getProfile`" must suppress "Possible null dereference: `user` may be undefined in getProfile" in the same file.
   - The same rejection must **not** suppress "SQL injection: `name` is concatenated into the query in getProfile" in the same file.
   - The same rejection must **not** suppress the null-dereference text in a file with a different extension.
   - A 👎 from a user with only read permission is ignored.
   - A thread resolved **with** a later commit touching the lines is not a rejection.
   - A rejection followed by a maintainer's "good catch" reply is not a rejection.
   - Running the collection twice on the same API fixtures gives a byte-identical `rejected.json`.

8. **Docs: `docs/learn.md`.**
   - What counts as feedback, who counts, what is stored, how to turn it on, and how to reset it (delete `feedback/rejected.json`).
   - A **scripted demo** for the demo repo: open a PR → TraceHound comments → a maintainer 👎s one comment → push a commit → the same comment is not posted again, and the body says it was suppressed.
   - State plainly: "This changes what is posted; it is not measured by the benchmark."

## Rules
- Touch only these:
  - `packages/analyzer/src/learn/`;
  - `packages/analyzer/test/learn.test.ts` and `packages/analyzer/test/fixtures/learn/`;
  - one line in `packages/analyzer/src/bin.ts`;
  - `packages/action/src/learn.ts`;
  - `packages/action/src/action.ts`, only the post-filter hook and the footer field;
  - `packages/action/src/data.ts`, only the feedback update kind;
  - `packages/action/test/`;
  - `.github/actions/tracehound-review/action.yml`, only the `learn` input;
  - `docs/learn.md`;
  - `docs/action/README.md`, only one short section.
- No new dependencies. No model or embedding calls.
- No force-push, no merge. Open the PR to main **as a draft** titled `learn-v1: suppress comments maintainers rejected (S8)`. Wait for CI, and make one fix attempt if it's red.

End with: the PR number, the head sha, one line per commit, the test count, the fixture table (case → expected → got), and whether `learn: 'off'` output is byte-identical.
