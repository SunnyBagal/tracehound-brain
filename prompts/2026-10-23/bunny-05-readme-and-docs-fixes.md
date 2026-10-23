# bunny-05: bring #54's README up to date, and fix two docs claims the audit found

Tool: **Bunny**, a **new `claudeb` session** in `~/Projects/TraceHound-claude2`. Paste everything below the line. No `.env`; make no Nebius calls.

---

Two docs-only jobs that don't need the 26 Oct numbers. The Action is frozen (`review-freeze-2` → `0aeef06`); docs changes are allowed.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin --tags`; `git status --porcelain` is empty. `origin/main` contains `5fb8cdc` (#59).
- In a plain shell, the `GIT_*` env check prints nothing. `pnpm test` and `pnpm typecheck` are green. **If the only failures are `Test timed out in 5000ms`**, rerun just those files with `--testTimeout 30000` (from the package folder: `pnpm exec vitest run <file> --testTimeout 30000`). If they pass, note it and continue. Bunny-04 hit one such flake today.
- Read:
  - `README.md` on main, and #54's branch `docs/judges-readme` (`db83286`);
  - `docs/action/README.md` (around lines 165–176);
  - `docs/contracts.md` (around lines 10–16);
  - `.github/actions/tracehound-review/action.yml`;
  - `packages/action/test/review-docs.test.ts`, which pins phrases in `docs/action/README.md`; keep them;
  - `docs/devpost/writeup.md` on #58's branch `docs/devpost-drafts`;
  - decision 068 in `docs/decisions.md`;
  - the codex-13 audit summary below.
- **Don't open** `linea-seeded`, `recall-seeded`, `tracehound-seeds`, ReviewBench `golden/`, or anything Cal.com.

## Job A: #54 (branch `docs/judges-readme`): everything except the numbers
First merge `origin/main` into the branch (`git merge --no-ff origin/main`, a merge commit, never a rebase or force-push). Then, one commit each:
1. **Install block:**
   - the pin becomes `SunnyBagal/TraceHound/.github/actions/tracehound-review@review-freeze-2`;
   - **remove** the `model:` line, because the default is now `nvidia/Nemotron-3.5-Super-VL` (068);
   - the comment around it says the default runs every review stage on Nemotron 3.5 Super VL, and fixes on Nemotron 3 Super.
2. **Contracts are merged** (#53): drop "landing in int-01", "draft PR #53" and the "Merge cross-file contracts" item under what's next, in the text and in the Mermaid node.
3. **Model story:** anything that says 3.5 "is being evaluated" becomes 067's result, in decision 066/067 wording:
   - "adopted on 2 dev runs: v1 F 50.4 vs 42.3; the second judge's interval includes 0";
   - then 068: default, with partial reviews when a chunk fails.
4. **Remove the duplicate `# TraceHound` H1** (the second one, in Details). Check that the Mermaid block is valid: render it with `npx -y @mermaid-js/mermaid-cli` if it installs, or else check the syntax by eye and say which.
5. **Keep every `[[RESULTS-TABLE]]`, `[[REVIEWBENCH]]`, `[[SEEDED-TEST]]` and `[[COST]]` placeholder.** Add `[[CALCOM-V2]]` next to the results placeholder, with a one-line note: "decision 069: v2 on Cal.com, the second look".

**Wording rules:** never "beats", never "3rd of 6" as a ranking, no competitor names outside the results section.

Push to the branch. #54 stays a draft.

## Job B: new branch `docs/contracts-boundary` from `origin/main`: correct two claims (codex-13 audit)
The audit found that a PR's `tsconfig.json` (`extends`, `paths`) can make the compiler read files **outside the checkout**, and a finding's text can quote them. tsconfig `plugins` and config packages were read as JSON and `.d.ts` only, and **no PR JavaScript ran**. The Action reviews **same-repo PRs only** (forks are skipped in `action.ts`). A same-repo author already has write access, and their branch's workflow runs with the repo's secrets.

One commit each:
1. `docs/action/README.md` (~line 170): replace "executes no script from the PR's code" with text that says exactly this:
   - it doesn't install dependencies or run the PR's scripts, and it loads no TypeScript plugins;
   - it does parse the PR's `tsconfig.json` and type files with TraceHound's bundled TypeScript, and those can reference files outside the checkout;
   - so contracts, like the rest of the Action, are for same-repository PRs only: fork PRs are skipped, and the stage is not a sandbox.
   - Keep every phrase `review-docs.test.ts` pins.
2. `docs/contracts.md` (~line 13): "symlinked into both worktrees read-only" becomes "symlinked into both worktrees; the symlinks don't make them read-only, and the analyzer only reads them".
3. Add a short **"Security boundary"** subsection to `docs/action/README.md`: same-repo PRs only; forks skipped in code; why; what the contracts stage reads; and that hardening (restricting compiler reads to the checkout, an env allowlist) is backlog.

Open a **draft** PR `docs: contracts security boundary (codex-13 audit)`.

## Job C: #58 (branch `docs/devpost-drafts`): one sentence
In `docs/devpost/writeup.md`, replace "no script from the pull request is run" (and any equivalent) with the same bounded wording as Job B. Note the sentence again under checklist item 11 or wherever claims are listed. Push to the branch; #58 stays a draft.

## Rules
- Docs only. **Don't touch:** `packages/`, `scripts/`, `config/`, `.github/`, `viewer/`, `docs/bench/results.md`.
- No invented numbers.
- Run the gates as their own commands before each commit.

## STOP AND REPORT
Final message:
- for each of #54, the new PR and #58: number, head sha, and one line per commit;
- test counts (and any timeout rerun);
- how the Mermaid block was checked;
- anything you couldn't do.

Also write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-23-bunny-05.md` (rules in `inbox/README.md`).
