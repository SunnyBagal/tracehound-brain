# codex-16: README first screen for judges (PR #54), from codex-15's findings

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Paste everything below the line. Start it **after Sunny has merged #66** (the README's `#7-precision-levers…` anchors resolve only then).

---

PR #54 (branch `docs/judges-readme`) is the README judges will read. A judge stand-in (codex-15) followed it from a fresh clone and found the first screen asks for a paid key before showing anything. Fix the first screen. **Docs only, $0, no model calls.**

## Step 0 (STOP and report on any failure)
- `git fetch origin`; `git status --porcelain` empty; `git switch docs/judges-readme`; it equals `origin/docs/judges-readme` (head `2bb3144` or a later commit that only touched README.md; if something else changed, STOP).
- `git merge origin/main` (no conflict expected; STOP on any). Main must contain PR #66 (`git log origin/main --oneline | grep -i "fill-01\|#66"`), else STOP.
- Read `~/Projects/tracehound-brain/submission/numbers.md` §0 (wording rules) and §1, §2, §8. **Every number you write comes from there; add no new number.**

## Edits to `README.md` only (one commit)
1. **Right under the one-line description, before "Try it"**, a short "See it without a key" block (≤ 6 lines):
   - the dashboard on its own line: `https://tracehound-tau.vercel.app` (reviews as bug cards, the benchmark page, the component graph);
   - the demo repo link (already in the README);
   - one results sentence: numbers.md §1's allowed sentence, then a link to `docs/bench/results.md`;
   - the local route: `pnpm install`, `pnpm fixture`, `pnpm test` need no key (link the Quick start section).
2. **"Try it" intro** (before the YAML): it needs a Nebius Token Factory key as a repository secret and runs on **same-repository** pull requests only (forks get no secrets: move that sentence up from after the YAML); cost about $0.06–0.07 per pull request on 3.5, pre-VAT (numbers.md §8); a review is partial when a chunk returns invalid JSON (about 7 of 40 dev pull requests per run), and the review says so.
3. **Quick start, `pnpm demo`:** add that without `NEBIUS_API_KEY` it doesn't stop: it uses heuristic component names and overwrites the committed `snapshots/`; restore them with `git checkout -- snapshots/`.
4. Don't touch anything else, including the Results, Losses and What's next sections, and **leave both `[[TBD-SEEDED]]` exactly as they are**.

## Checks (report each)
- `grep -c "TBD-SEEDED" README.md` = 2.
- No "beats", "best", "outperforms", "state of the art" added (`git diff origin/docs/judges-readme -- README.md`).
- Every relative link and `#anchor` in README.md resolves on the merged tree (reuse `~/Projects/TraceHound-judge/_judge/links.py` if it's there).
- Push to `docs/judges-readme`. Don't mark the PR ready.

## Report (final message)
Head sha, the diff of the first 60 lines of README.md, the check results. Also write it to `~/Projects/tracehound-brain/inbox/2026-10-27-codex-16.md` (`git pull --rebase`, add only that file, commit `inbox: codex codex-16 report`, push).
