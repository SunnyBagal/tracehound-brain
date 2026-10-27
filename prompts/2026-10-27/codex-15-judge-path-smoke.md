# codex-15: judge-path smoke — follow the README from a fresh clone, as a judge would ($0, report only)

Tool: **ChatGPT Codex**, a **new task**, in a **new folder `~/Projects/TraceHound-judge`** (not `TraceHound-codex`, not `TraceHound`, not `TraceHound-ui`). Paste everything below the line.

**Sunny, once, in a plain terminal:**
```
cd ~/Projects && git clone https://github.com/SunnyBagal/TraceHound TraceHound-judge && cd TraceHound-judge && git fetch origin docs/judges-readme
```
Then open that folder in Codex. No `.env` in it; don't copy one in.

---

You are a hackathon judge's stand-in. A judge opens `https://github.com/SunnyBagal/TraceHound`, reads the README and tries what it says. Your job: **do exactly that from a fresh clone and report every place it breaks, misleads or doesn't match.** **Report only: no commits, no branches, no PRs, no edits to tracked files.** Scratch files go in `./_judge/` (untracked).

The README a judge will see is the draft on branch `docs/judges-readme` (PR #54), not main's. Save it: `git show origin/docs/judges-readme:README.md > _judge/README.md` and follow **that** file. Main is `5ba60a0` or later.

## Hard rules
- **$0: no model calls.** Run every command with the keys removed: prefix each with `env -u NEBIUS_API_KEY -u NVIDIA_API_KEY`. A step that needs a key is **not run**: report it as "needs a Nebius key (not run)" and check only that, without a key, it fails fast with a clear message (one attempt, then stop that step).
- Don't run anything that writes outside `~/Projects/TraceHound-judge`. Don't touch `~/Projects/TraceHound`, `TraceHound-ui`, `TraceHound-codex`, `tracehound-bench`, `tracehound-ops` or `tracehound-brain` (except the inbox file at the end).
- Never open Cal.com data, ReviewBench `golden/`, `linea-seeded`, `recall-seeded` or `tracehound-seeds`.
- If a step needs a tool that isn't installed (e.g. Docker not running), report it as a judge prerequisite the README does or doesn't state; don't install system software.

## Checks, in order (record command, exit code, wall time, and the first error line for each)
1. **Prerequisites** as the README states them (Node version, pnpm version). Record `node -v`, `pnpm -v`. If the README's Node version differs from what's installed, run with what's installed and say so (CI uses Node 22: note any mismatch between README, `package.json` `engines`, `.nvmrc`/`.node-version` if present, and `.github/workflows/*`).
2. **Quick start**, literally: `pnpm install` (as written; then note whether `--frozen-lockfile` was needed), `pnpm fixture`, `pnpm test`, then `pnpm typecheck`. Test failures that are only 5 s timeouts: rerun those files with `--testTimeout 30000` and report both. Record the final pass/skip/fail counts.
3. **Viewer**: `pnpm --filter @tracehound/viewer build`; then serve it (the README's dev command, or a static server on `viewer/out`), and `curl -s -o /dev/null -w "%{http_code}"` each of `/`, `/graph`, `/reviews`, `/benchmark`. Kill the server after.
4. **`pnpm demo`** (key removed): does it fail fast with a clear message, or start writing to `snapshots/` first? Afterwards `git status --porcelain snapshots/` must be empty; if it isn't, report the files and restore them with `git checkout -- snapshots/`.
5. **Repair harness (toy task)**: build the toy repo as the README says, then the `--agent noop --provider docker` command only (no model). If Docker isn't available, report it.
6. **The Action block in "Try it"**: (a) `0aeef06fa8742cc8a36581c3894e554b8748c965` is the commit tagged `review-freeze-2` (`git rev-list -n 1 review-freeze-2`); (b) `.github/actions/tracehound-review/action.yml` at that commit accepts every input the block passes (`nebius-api-key`, `model`) and the default of every input it doesn't; (c) the YAML parses (`node -e` with any YAML parser already in `node_modules`, or `python3 -c "import yaml"` if available); (d) compare it with the example it says it comes from (`docs/action/example-workflow.yml`) and list real differences.
7. **Links in `_judge/README.md`**: every relative link resolves to a file in the tree at `origin/main` **or** at `origin/docs/judges-readme`; every `#anchor` into a markdown file matches a heading there (GitHub's slug rules). Report broken ones with line numbers. External `https://` links: `curl -sI -L` each once; report non-2xx (network may be blocked in your sandbox: then say so and skip).
8. **The live dashboard** `https://tracehound-tau.vercel.app`: `/`, `/reviews`, `/benchmark`, `/graph` return 200 (skip if no network). Note whether `/benchmark` still shows v1 numbers (expected until a later redeploy; just report).
9. **Read as a judge**, in ≤ 10 bullets: what's confusing or missing in the first screen of the README (what it is, how to try it without a key, where the results are), with line numbers. No rewrites, just findings.

## Report (final message)
A table: check · result (pass / fail / not run) · evidence (command, exit code, first error line). Then the ≤ 10 judge-reading bullets. Then the exact counts from step 2. Keep it under 120 lines.

Also write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-27-codex-15.md`, then in that folder `git pull --rebase`, `git add inbox/2026-10-27-codex-15.md`, commit `inbox: codex codex-15 report`, `git push`. If you can't write or push there, say so at the top of your final message (Sunny will paste it to the chat).
