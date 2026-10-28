# Codex jobs for 29 Oct (submission day), while Claude is at its weekly limit until 18:30 IST

Four Codex jobs, each a **new Codex task** unless it says otherwise, in the folder named for it. Paste only the part below each job's line. Every job: docs or config only, **$0, no model calls**, never print a key. Report to the brain inbox (`~/Projects/tracehound-brain/inbox/<file>`: `git pull --rebase`, add only that file, commit, push).

Order: **codex-19** now → (Sunny merges #54, #58) → **codex-21** when Sunny's answers file exists → (Sunny merges #65) → **codex-20** → (optional) **codex-22**.

---

## codex-19: ui-11c on PR #65 (instead of the Claude UI session)

Folder: **`~/Projects/TraceHound-ui`** (the worktree on `ui/v2`; the Claude session there must be idle). New task.

---

You're finishing PR #65 (`ui/v2`) of TraceHound's viewer. Docs/config/UI only, $0.

Step 0 (STOP on failure): `git fetch origin`; `git status --porcelain` empty; current branch `ui/v2` equals `origin/ui/v2`. Read `CLAUDE.md` (Gates).

One commit:
1. **Trademark line.** Under the landing hero (`viewer/components/landing/Hero.tsx` or the landing page that renders it), one small muted line: "Logos are trademarks of their owners and are used only to name the coding agents whose pull requests TraceHound reviews. No affiliation or endorsement." Add the same sentence to `CREDITS.md`, with the list of marks the hero shows (from `MARKS`). Keep every mark as it is (the owner's decision).
2. **Seeded lane on:** `viewer/lib/repos.ts`: `PR_LANE_FLAGS.recallSeeded: true` (the seeded test was scored on 28 Oct; those PRs are public results). Update any test that asserts it is off; don't change tests that assert data.

Gates, each its own command, read each: the `GIT_*` env check from CLAUDE.md, `pnpm --filter @tracehound/viewer test`, `pnpm --filter @tracehound/viewer typecheck`, `pnpm --filter @tracehound/viewer build`. Then `git push origin ui/v2`. Don't merge.

Report: head sha, files changed, gate results. Inbox: `2026-10-29-codex-19.md`.

---

## codex-21: fill Sunny's answers into the Devpost text

Folder: **`~/Projects/TraceHound-codex`**. New task. Start when `~/Projects/tracehound-brain/inbox/2026-10-29-sunny-answers.md` exists and #58 is merged.

---

Sunny wrote answers for the `[[SUNNY: …]]` slots in TraceHound's Devpost drafts. Put them in, verbatim apart from typo fixes. Docs only, $0.

Step 0 (STOP on failure): `cd ~/Projects/tracehound-brain && git pull --rebase`; read `inbox/2026-10-29-sunny-answers.md`. In `~/Projects/TraceHound-codex`: `git fetch origin`, clean tree, branch `docs/sunny-answers` from `origin/main`; `git grep -n "\[\[SUNNY" origin/main -- docs/devpost README.md` lists the slots.

1. Replace each `[[SUNNY: …]]` that Sunny answered (Q1 personal reason in `writeup.md` "Inspiration"; Q6 / Q7 in `what-changed-since-26-aug.md`; Q10 in `checklist.md`; Q5 the YouTube URL in `writeup.md` "Links", only if he gave it). Keep the slot for any question he didn't answer.
2. Fix only typos and grammar in his text; don't add claims. If an answer contains a number or a claim about results, it must match `~/Projects/tracehound-brain/submission/numbers.md`, or STOP and report it.
3. No competitor or tool names in `writeup.md` (Cursor, Augment, Greptile, CodeRabbit, Copilot, Bugbot): `git grep -n -i -E "cursor|augment|greptile|coderabbit|copilot|bugbot" -- docs/devpost/writeup.md docs/devpost/video-script.md` prints nothing.
4. Commit, push, open a PR titled `docs: Sunny's answers in the Devpost drafts`, ready (not draft). Don't merge.

Report: PR number, head sha, each slot old → new, slots left. Inbox: `2026-10-29-codex-21.md`.

---

## codex-20: submission pre-flight audit (report only, plus the repo description)

Folder: **`~/Projects/TraceHound-judge`** (the fresh clone from codex-15). New task. Start after #54, #58 and #65 are merged.

---

TraceHound (Nebius × NVIDIA hackathon) is submitted tonight. Audit what judges will see. **Report only** except item 9. $0, no model calls, keys removed from every command (`env -u NEBIUS_API_KEY -u NVIDIA_API_KEY`).

Step 0: `git fetch origin --tags && git checkout -q origin/main`. `git log --oneline origin/main` contains the merges of #54, #58, #65, #66, #67, #68 (list each one's merge sha; a missing one = finding).

Checks (record command, result, first error):
1. **Placeholders:** `git grep -n -E "\[\[(TBD|SUNNY|UI)" origin/main -- README.md docs/devpost` — every hit listed (Q5's YouTube URL may still be open: say so).
2. **Numbers:** every number in README.md's "See it without a key", "Results", "Seeded", "Losses", "Cost" sections and in `docs/devpost/writeup.md` appears in `~/Projects/tracehound-brain/submission/numbers.md`. List any that don't, with line numbers. Don't judge wording; just mismatches.
3. **Names:** `git grep -n -i -E "cursor|augment|greptile|coderabbit|copilot|bugbot" origin/main -- docs/devpost/writeup.md docs/devpost/video-script.md` prints nothing.
4. **Wording:** no "beats", "best", "outperforms", "state of the art" in README.md or writeup.md (except a sentence listing banned words).
5. **Pins:** README "Try it" and `docs/devpost/video-script.md` pin `86fd5d796c901cb20177cda28c4925e8c72d0901`; `git rev-list -n 1 review-freeze-2.1` equals it; `SunnyBagal/tracehound-demo`'s workflow pins it (`gh api repos/SunnyBagal/tracehound-demo/contents/.github/workflows --jq '.[].name'`, then read each).
6. **Links:** every relative link and anchor in README.md and `docs/devpost/*.md` resolves on `origin/main` (reuse `_judge/links.py`); every `https://` link: one `curl -sI -L`, non-2xx listed.
7. **Live site** `https://tracehound-tau.vercel.app`: `/`, `/graph`, `/reviews`, `/benchmark` return 200. `/benchmark` links the same results revision as `viewer/lib/benchmark.ts`'s `BENCHMARK_REVISION` on main. `/` contains the trademark line. Vercel's latest production deployment is from main's head (`gh api repos/SunnyBagal/TraceHound/deployments --jq '.[0] | .sha, .environment'`).
8. **Public repos:** `SunnyBagal/TraceHound`, `tracehound-demo`, `recall-seeded`, `linea-seeded`, `tracehound-seeds` all `PUBLIC` (`gh repo view <r> --json visibility`); `https://github.com/SunnyBagal/tracehound-seeds/blob/main/RESULTS.md` returns 200.
9. **The one change you make:** the repo's About text is still pre-pivot. Run:
   `gh repo edit SunnyBagal/TraceHound --description "Open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on Nebius Token Factory. Pre-registered benchmarks; every loss published." --homepage "https://tracehound-tau.vercel.app"`
   and `gh repo edit SunnyBagal/TraceHound --add-topic nvidia-nemotron --add-topic nebius --add-topic code-review --add-topic github-actions`. Report the before/after description.
10. **Clean-clone gates:** `pnpm install --frozen-lockfile`, `pnpm test`, `pnpm typecheck` on main (counts).

Report: a table check → pass/fail → evidence, then a "must fix before submit" list (only real blockers) and a "nice to have" list. Inbox: `2026-10-29-codex-20.md`.

---

## codex-22 (optional, only if everything above is done): $0 truncation scan

Folder: **`~/Projects/TraceHound`** (Sunny's main checkout; has `.tracehound/cache`). New task.

---

Report only, $0, no model calls, no file changes. `git fetch origin && git status --porcelain` must be clean and main must contain `scripts/bench/truncation-scan.ts` (else STOP). Run `env -u NEBIUS_API_KEY -u NVIDIA_API_KEY node scripts/bench/truncation-scan.ts --cache .tracehound/cache --model nvidia/Nemotron-3.5-Super-VL` and the same for `nvidia/nemotron-3-super-120b-a12b`. Paste both outputs verbatim (counts only). Don't interpret beyond one line each. Inbox: `2026-10-29-codex-22.md`.
