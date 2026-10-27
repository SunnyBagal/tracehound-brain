# devin-03: the Action fails to install on repos that pin pnpm via `packageManager` — fix it as review-freeze-2.1

Tool: **Devin, the same session** (devin-01/02). Paste everything below the line.

---

The seeded test found a real bug in the shipped Action. On `SunnyBagal/linea-seeded`, every TraceHound run fails in the composite action's `pnpm/action-setup@v4` step with **`Error: Multiple versions of pnpm specified`**, before any model call. The action pins `version: 11.20.0`, and the reviewed repo's root `package.json` has `"packageManager": "pnpm@9.0.0"`. `pnpm/action-setup` reads the **reviewed repo's** `package.json` from the workspace, so any user repo that pins a different pnpm can't run TraceHound at all. That includes judges trying the README's "Try it".

**Do not open, clone or read `linea-seeded`, `recall-seeded` or `tracehound-seeds`** (a blind test is running on them). Everything you need is in this message.

## The fix: a new frozen release whose only change is the install step
The product every published number describes is the review pipeline at `review-freeze-2` (`0aeef06`). The new release must differ from it **only in `.github/actions/tracehound-review/action.yml`**, so the review code is byte-identical.

1. Branch **`release/review-freeze-2.1` from `0aeef06`** (not from main). One commit, touching only `.github/actions/tracehound-review/action.yml`. Install pnpm **without reading the workspace's `package.json`**. For example: run `actions/setup-node` first, then a bash step `npm install -g pnpm@11.20.0` (or corepack scoped to `${{ github.action_path }}/../../..`). Or keep `pnpm/action-setup` only if you can show from its source/docs that `package_json_file` resolves to the action's own `package.json` and not the workspace's. Pick the simplest one that you can prove. Keep `node-version: '24'`, the `--frozen-lockfile --ignore-scripts --filter @tracehound/analyzer...` install, and every later step byte-identical.
2. **Proof, on main** (a separate PR to `main`, branch `devin/action-install-fix`): cherry-pick the same action.yml change, plus a workflow `.github/workflows/action-install-matrix.yml` (`workflow_dispatch` + `pull_request` on paths `.github/actions/**`). It checks out TraceHound into a subfolder (e.g. `th/`), writes a root `package.json` in the workspace for each matrix case, and runs `uses: ./th/.github/actions/tracehound-review` with the event **not** `pull_request`, so the review step is skipped: **install steps only, no key, no model call**. Matrix: `"packageManager": "pnpm@9.0.0"`, `"pnpm@10.0.0"`, `"pnpm@11.20.0"`, `"yarn@4.5.0"`, `"npm@10.8.0"`, and no `packageManager`. Show it **fails for pnpm@9 on the old action.yml** (one run before the fix, linked) and **passes all six after**. If the event guard makes `uses:` impossible to exercise that way, say why and test the setup steps' commands directly in the same layout.
3. `git diff 0aeef06 release/review-freeze-2.1 --stat` must list exactly one file. Paste it in the PR body.
4. On the main PR: `docs/decisions.md` gets **072 — review-freeze-2.1** (why: the seeded test's Linea runs failed at install; what changed: the install step only; review paths identical to `review-freeze-2`, `git diff` above; every result still describes `review-freeze-2`'s pipeline; the Action pin moves to the 2.1 commit). `docs/freeze.md`: one line noting 2.1. **Don't** change README.md (another PR owns it), `docs/bench/`, `viewer/`, any prompt or review code.
5. Push both branches. Open the main PR (ready, not draft), wait for CI and the matrix workflow. **Don't create the tag**: Sunny tags after the chat checks. Report the release commit's full sha.

## Rules
No model calls, no keys, $0. No new dependencies. Gates as before on the main PR (`pnpm test`, `pnpm typecheck`, separate commands). Never merge.

## Final message
The release commit sha and its one-file diff; the main PR number, head sha, CI state; the matrix run links (failing before, passing after); anything you couldn't prove.
