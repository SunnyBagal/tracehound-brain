# codex-10 (int-01): wire cross-file contracts into the Action

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. **Must land by 22 Oct**, when Codex ends. Paste everything below the line.

---

You're wiring the deterministic cross-file contract checker (`tracehound contracts`, PR #47) into the TraceHound GitHub Action. It finds things a PR changed that **other files depend on**: typed exports compared between base and head, and string keys such as routes, queue names, Redis keys, env vars and events. It makes no model call. On its fixtures it caught 10 of 10 planted breaks with 0 findings on 10 controls.

The Action today runs pool-verify-v1 and posts. This job adds contracts beside it. The gate (codex-08) and the evidence verifier are **not** wired: both lost on dev (decision 065). Don't add them.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` is empty. Branch `codex/int-01-contracts` from `origin/main`, which must contain #49 (`learn` input) and #51 (`review-missed`).
- `pnpm test` and `pnpm typecheck` are green.
- Read:
  - `packages/action/src/action.ts`, `pipeline.ts`, `cli.ts` and `data.ts`;
  - `packages/analyzer/src/contracts/cli.ts`, and the contract report's types in `packages/analyzer/src/contracts/`;
  - `docs/action/README.md`;
  - `.github/actions/tracehound-review/action.yml`.
- **Don't open** `SunnyBagal/linea-seeded`, `recall-seeded` or `tracehound-seeds`.

## What to build (one commit per item, each with tests)
1. **New input `contracts`**, `'on' | 'off'`, **default `'on'`**. With `'off'`, the posted output is **byte-identical** to today's (add a test).
2. **Run it.** When on, the review step runs `tracehound contracts --repo <checkout> --base <merge-base of base and head> --head <head sha> --out <tmp> --budget-s 90`:
   - **concurrently** with the reviewer pipeline;
   - with the **merge base**, not the base tip (lesson from #43). If the checkout is shallow, fetch the base commit **without credentials**, the same way the data job already does.
   - **Fail-soft:** a timeout, error or invalid output logs one line (`contracts: failed (<reason>)`) and the review posts as it would with `'off'`. Contracts must never block or fail a review.
   - **Never install dependencies or run any script from the PR's code.** The checker reads files only.
3. **Deliver.** Each contract finding is placed by where it points:
   - On a line that is commentable in the diff (use the existing `newSideLines` / diff index): an **inline comment**, with the body prefixed `Contract:`.
   - Anywhere else, usually a file the PR didn't edit: **one section in the review body**, headed `Files you didn't edit that depend on this change`, with up to 10 entries as `path:line`, linked to the blob at the head sha, each with a one-line reason. If there are more, add "and N more".
   - Contract findings **don't count against `max-comments`** for model findings, but cap them at 10 inline. Dedupe a contract finding against a model finding on the same path, within ±3 lines (keep the contract one).
   - `learn` suppression (#49) does **not** apply to contract findings.
4. **Footer record:** add `contracts: { status: "ok"|"failed"|"off", inline: n, body: n, ms: n }`.
5. **Docs** (`docs/action/README.md`, one section): what contracts checks, that it's deterministic, how to turn it off, and its time budget.
6. **Live check (required before you finish):** on the demo repo the Action already targets (see `docs/action/README.md`), open one PR **pinned to this branch's head sha** that renames an exported function used by another file the PR doesn't touch. Report the review's URL, whether the body lists the dependent file, and the run time. Spend: one normal review (cents).

## Rules
- Touch only these: `packages/action/src/` (`action.ts`, `cli.ts` and one new `contracts.ts` helper), `packages/action/test/`, `.github/actions/tracehound-review/action.yml` (the input and its env line), and `docs/action/README.md`.
- **Nothing in `packages/analyzer/`.**
- Keep #43's git rules (`cleanGitEnv`; the token only on branch git).
- No new dependencies. No force-push, no merge. Open the PR as a **draft** titled `int-01: cross-file contracts in the Action`. Wait for CI; make one fix attempt if it's red.

End with: the PR number, head sha, one line per commit, test counts, the byte-identical result for `contracts: 'off'`, the live-check URL and what it showed, and anything you couldn't do.
