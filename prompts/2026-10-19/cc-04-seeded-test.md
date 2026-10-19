# cc-04: the seeded cross-file test (answer key + seed branches; no PRs yet)

Tool: **Claude cloud** (claude.ai/code). Start a **new session** on `SunnyBagal/tracehound-seeds` (private). It spends cloud credit only; no Nebius calls. Paste everything below the line.

**Sunny, before starting (Terminal on your Mac):**
```bash
gh repo create SunnyBagal/tracehound-seeds --private --add-readme
gh repo create SunnyBagal/linea-seeded --public
gh repo create SunnyBagal/recall-seeded --public
cd /tmp && rm -rf l r
git clone https://github.com/SunnyBagal/Linea l && git -C l push https://github.com/SunnyBagal/linea-seeded.git main
git clone https://github.com/SunnyBagal/Recall r && git -C r push https://github.com/SunnyBagal/recall-seeded.git main
```
Only `main` is pushed, on purpose; never push Recall's other branches. Then give the Claude GitHub app access to all three repos.

---

You're building a **pre-registered test set** for AI pull-request reviewers: seeded bugs, an answer key, and controls. The question it answers is: **do reviewers catch breaks in files the PR did not edit?**

## Blindness (read first)
- **Do not open the `SunnyBagal/TraceHound` repo at all.** In particular, never look at any `contracts`, `gate` or `review` code or docs. The seeds have to be written without knowledge of any reviewer's detection rules. Use only general knowledge of how real changes break code.
- Plant **new** bugs only. Don't search for, describe or fix bugs that already exist in Linea or Recall.
- Never mention "seed", "bug", "test", "benchmark" or "TraceHound" in any branch name, commit message, PR title, PR body or code comment on the public mirrors. Those must read like ordinary developer changes.

## Step 0 (if any check fails, STOP and report)
- Attach `SunnyBagal/linea-seeded` and `SunnyBagal/recall-seeded` to this session. Each one's `main` must equal its source repo's `main`.
- Install each repo's dependencies in your sandbox and run its typecheck and tests on `main`. Record the result, since this is the baseline every seed is compared against.

## The 30 seeds
Each seed is one branch off the mirror's `main`, with one commit, a realistic diff of 5–150 lines, and a PR title and body that don't hint at the defect. Split them roughly evenly between the two repos, wherever the pattern fits the code.

| Class | n | What it is | Must be true |
|---|---|---|---|
| **X-typed** | 6 | Cross-file break the compiler sees in an **unedited** file: a required param added, an exported field renamed, a return type made nullable with a dereference elsewhere, an enum member removed, an export renamed with a stale importer, a shared protocol type changed | Typecheck fails **only** in files the commit did not touch |
| **X-keyed** | 6 | Cross-file break through a **string**: a ws message type renamed on one side, an env var renamed in config only, a route path changed on the server only, a Redis key prefix changed in the writer only, a queue or job name changed in the producer only, an event or storage key renamed on one side | Typecheck and tests pass; the stale use is in an unedited file |
| **X-semantic** | 4 | Cross-file break with **no shared literal and no type error**: units changed (ms → s) for a value read elsewhere; two same-typed params swapped with unedited callers; a default changed so callers now get a different behaviour; a function that returned null now throws, and callers don't catch | Typecheck passes; the broken behaviour is in an unedited file |
| **D** in-diff | 6 | Ordinary bugs inside the diff: off-by-one, a missing `await`, a wrong comparison, an unhandled null, unsafe query or HTML building, a check-then-act race | Typecheck passes |
| **C** control | 8 | Clean changes that touch shared things **correctly**: a rename done everywhere, an optional param added, a file moved with every import updated, a key renamed on both sides, a refactor with no behaviour change, a dependency-free small feature | Typecheck and tests pass; no defect |

Branch names look like a normal developer's (`feat/...`, `fix/...`, `refactor/...`), numbered privately in the key.

## Files in `tracehound-seeds` (the private repo)
- `key.json`: one entry per seed:
  ```
  {id, repo, branch, class, title, summary,
   accept: [{path, lines: [start, end]}],
   brokenSymbolOrKey, edited: [paths], why}
  ```
  - `accept` lists where a correct comment would point: the stale use in the unedited file (classes X), or the defect line (D). It's empty for C.
- `prs/<id>.md`: the PR title on the first line, then the body.
- `SCORING.md` (pre-registered; copy this exactly):
  - A seed counts as **caught** by a tool if either:
    - (a) any inline comment by that tool is on an `accept` path, at a line within `[start-3, end+3]`; or
    - (b) its review body or summary names an `accept` path **and** `brokenSymbolOrKey`.
  - Each tool's caught/missed status is decided by `score.ts` for (a). Sunny decides (b), and every (b) call is listed with the comment quoted.
  - Report, per tool and per class: caught / n. Also report the comments on C controls (Sunny marks each valid or invalid) and the extra comments on seeded PRs.
  - No LLM judge.
  - Tools: TraceHound (frozen at freeze-2), Greptile, and Bugbot. CodeRabbit is excluded: its terms forbid publishing benchmark results.
- `verify.ts`: for every seed, it checks the branch has one commit on `main`; that the typecheck result matches the class rule above (and for X-typed, every error is outside `edited`); that tests pass where the rule says so; that `accept` lines exist at the branch head; and that the no-hint words don't appear on the public branch. It prints a table, and every row must pass.
- `open-prs.sh`: Sunny runs this himself on 25 Oct. For each seed it runs `gh pr create --repo <mirror> --head <branch> --base main --title … --body-file prs/<id>.md`, in a shuffled order with a seed recorded in the script. Write it, but **don't run it.** Bugbot only reviews PRs its user authored, so the PRs must come from Sunny's account, after the reviewers are installed.
- `score.ts`: reads each PR's review comments through the GitHub API (`GITHUB_TOKEN` from env) and `key.json`, then prints the (a) table. Test it on fixtures only.
- `README.md`: what this is, the blindness rules, and how to run it.

## Push
- The seed branches go to the public mirrors. **No PRs.**
- Everything else goes to `tracehound-seeds` `main`.
- End by printing `sha256sum key.json SCORING.md`. Sunny records those hashes publicly as the pre-registration, without revealing the key.

## Rules
- No force-push, no merge, no PRs on the mirrors, no Nebius or model API calls.
- Don't change the mirrors' `main`. Don't add or modify CI workflows on the mirrors.

End with: the `verify.ts` table (30 rows, all passing), the class counts per repo, the two sha256 hashes, and the `tracehound-seeds` head sha.
