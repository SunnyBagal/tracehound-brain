# codex-13: read-only audit. What can a hostile PR do to the contracts stage?

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Paste everything below the line. **Read-only: no branch, no commit, no PR, no Nebius call.**

---

Since #53 (int-01), the TraceHound GitHub Action runs the **contracts analyzer by default on every same-repo PR**. It checks out the PR head, builds git worktrees at the merge base and the head, and runs TypeScript's compiler API (`packages/analyzer/src/contracts/`) over files the PR author controls. Fork PRs are skipped (`packages/action/src/action.ts`), so the author already has write access. The risk is lower than in an open-fork setup, but not zero: a write-access contributor, or a compromised branch, still shouldn't be able to make the reviewer leak the `GITHUB_TOKEN` or `NEBIUS_API_KEY`, read files outside the checkout, hang the job, or post content we didn't intend.

Ultrareview #2 checked process handling, budgets and escaping (all fixed in codex-11). Nobody has checked **hostile repository content**. That's this job.

The Action is frozen (`review-freeze-2` → `0aeef06`). This audit decides whether anything is serious enough to break the freeze. Most findings will go to the backlog.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin --tags`; `git status --porcelain` is empty; check out `review-freeze-2` (detached is fine). `git rev-parse HEAD` is `0aeef06fa8742cc8a36581c3894e554b8748c965`.
- `pnpm install --frozen-lockfile`; the `GIT_*` env check prints nothing; `pnpm test` and `pnpm typecheck` are green.
- **Don't open** `linea-seeded`, `recall-seeded`, `tracehound-seeds`, ReviewBench's `golden/`, or anything under `~/Projects/tracehound-bench/test/`.

## Read
- `packages/action/src/contracts.ts`, `action.ts`, `cli.ts`;
- `.github/actions/tracehound-review/action.yml` (env, steps, what runs where);
- `packages/analyzer/src/contracts/` (all files), `packages/analyzer/src/git-env.ts`;
- `docs/contracts.md` and `docs/action/README.md`.

## Questions (answer each with file:line evidence and, where possible, a local reproduction)
Build every reproduction as a **throwaway git repo under `/tmp`** with fixtures you write. Run the analyzer CLI (`node packages/analyzer/src/bin.ts contracts …`, or whatever `contracts.ts:129` calls) or the Action's contracts function against it. No network, no real tokens. Put dummy values in env vars named like the real ones and check whether they can be read.

1. **tsconfig `extends`:** can a PR's `tsconfig.json` `extends` a path outside the checkout (`../../../../etc/…`, an absolute path, `node_modules/…` that the PR also adds)? What does the analyzer read, and can any of it reach a finding's text, a log line or the posted review?
2. **tsconfig `compilerOptions.plugins`, `typeRoots`, `types`, `paths`, `rootDirs`, `references`:** does the compiler API **load or execute** anything from them (language-service plugins, `require` of a path)? Is any code from the PR ever executed?
3. **Symlinks:** a PR adds a symlink (`src/a.ts → /proc/self/environ`, `→ ~/.gitconfig`, `→ ../../../runner/_temp/…`). Does the worktree keep it, and does the analyzer follow it? Can the target's content reach findings, logs or the review body?
4. **Size and shape:** a 50 MB `.ts` file, 100,000 files, deeply nested types, a type that makes `tsc` explode. Does `--budget-s` bound it? (Known: the budget covers only the typed check; keyed and dependents are unbounded. Measure how bad it gets.) Does the job hang until the 20-minute workflow timeout?
5. **Text that reaches the review:** paths, export names, route/env/event keys. After `escapeContractText`, can a PR still inject a link, an @mention, an HTML comment that breaks the `tracehound:v1` footer, or text that looks like a TraceHound finding?
6. **Env and credentials:** what env does the analyzer child process get? Do `GITHUB_TOKEN`, `NEBIUS_API_KEY`, `ACTIONS_*` reach it? Is the checkout's `.git/config` credential-free when the analyzer runs (`persist-credentials: false` in the workflows, `extraheader` reset)?
7. **git itself:** a PR that adds `.gitattributes` filters, `.gitmodules`, or a `.git` file in a subdirectory. Can the merge-base fetch or the worktree creation run a filter, a hook or a submodule command?
8. Anything else you find on the way.

## Report (STOP AND REPORT; this is the final message)
A table, one row per question: **verdict** (safe / issue / not reproducible), **severity** (critical: secret or file leak, or code execution; high: hang to timeout, or a review-content injection that reaches the PR; low: cosmetic), file:line evidence, reproduction steps (short), and the smallest fix you'd suggest (described, **not applied**).

Then:
- the commands you ran and their results (test counts);
- anything you couldn't check, and why.

Write the report verbatim to `~/Projects/tracehound-brain/inbox/2026-10-23-codex-13.md` (rules in `inbox/README.md`: only that file; `git pull --rebase`, commit `inbox: codex codex-13 report`, push). Delete the `/tmp` fixtures when done.
