# codex-17: move every product pin to review-freeze-2.1 (README #54, Devpost #58, demo repo)

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Paste everything below the line. Start **after** Sunny has pushed the tag `review-freeze-2.1` and merged #68.

---

Decision 072 (on main via #68): `review-freeze-2.1` = `86fd5d796c901cb20177cda28c4925e8c72d0901`, which is `review-freeze-2` (`0aeef06`) plus one commit changing only the Action's install step. Results still describe `review-freeze-2`'s pipeline; the Action users pin is now 2.1. Docs only, $0, no model calls.

## Step 0 (STOP on failure)
- `git fetch origin --tags`; `git rev-list -n 1 review-freeze-2.1` = `86fd5d796c901cb20177cda28c4925e8c72d0901`; `git diff --stat 0aeef06 review-freeze-2.1` lists only `.github/actions/tracehound-review/action.yml`; `origin/main` contains decision `072`.
- Read `~/Projects/tracehound-brain/submission/numbers.md` §0.5 (the wording).

## 1. README, PR #54 (`docs/judges-readme`; `git merge origin/main` first; on a README-only conflict keep the branch side, as in codex-16b; anything else: STOP)
- "Try it" YAML: `@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2` → `@86fd5d796c901cb20177cda28c4925e8c72d0901 # review-freeze-2.1`; rewrite the YAML comment above it: 86fd5d7 is the commit tagged `review-freeze-2.1`: the measured review pipeline of `review-freeze-2` (`0aeef06`) with only the install step changed (decision 072).
- Results intro / "Live demo": keep "the product … at tag `review-freeze-2` (`0aeef06`)" and add numbers.md §0.5's one sentence on 2.1 once, where the product config is first stated. "Live demo": the demo repo now pins 2.1.
- Leave `[[TBD-SEEDED]]` (2) untouched. Links/anchors still resolve.
## 2. Devpost, PR #58 (`docs/devpost-drafts`; same merge rule)
- `writeup.md` "Trying it without a key": "pinned to `0aeef06`" → "pinned to `review-freeze-2.1` (`86fd5d7`), the measured pipeline with an install fix". Add one bullet to "Challenges we ran into" (no names): **the seeded test caught an install bug in our own Action**: on a repo pinning another pnpm version via `packageManager`, the setup step failed before any model call; fixed as review-freeze-2.1, a one-file change, with the review code unchanged (decision 072).
- `video-script.md`: any `0aeef06` pin shown on screen → `review-freeze-2.1`.
- Leave every `[[TBD-SEEDED]]` / `[[SUNNY…]]` untouched; no competitor or tool names.
## 3. Demo repo `SunnyBagal/tracehound-demo`
- In `.github/workflows/` on the default branch, change only the TraceHound `uses:` line(s) to `@86fd5d796c901cb20177cda28c4925e8c72d0901 # review-freeze-2.1` (keep `model: 'nvidia/Nemotron-3.5-Super-VL'`). One commit `ci: pin TraceHound review-freeze-2.1 (decision 072)`, pushed with `gh`/git. **Don't open, close or touch any PR there (demo PR #6 is never merged).**

## Report (final message)
Head shas of #54 and #58, the demo repo commit, every replaced line (old → new), `grep -rn 0aeef06` results left in README/writeup/video-script (each one should be a deliberate "review-freeze-2" reference). Also to `~/Projects/tracehound-brain/inbox/2026-10-28-codex-17.md` (pull --rebase, add only that file, commit, push).
