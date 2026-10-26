# bunny-06: fill the judges' README (#54), the Devpost drafts (#58) and the video script from the numbers sheet

Tool: **Bunny**, i.e. Claude Code on Sunny's account B. Folder `~/Projects/TraceHound-claude2`. Start: `cd ~/Projects/TraceHound-claude2 && claudeb`, then paste everything below the line.

---

You are finishing the written submission for TraceHound (Nebius × NVIDIA hackathon, deadline 30 Oct 22:30 IST, target submit 29 Oct). Two draft PRs exist: **#54** (`docs/judges-readme`, README for judges, with placeholders) and **#58** (`docs/devpost-drafts`, `docs/devpost/*`: writeup, video script, checklist, feedback summary, what-changed). Your job: put the final numbers and wording into both, from **one source only**. **No model calls, $0, docs only.**

## The source of truth
`~/Projects/tracehound-brain/submission/numbers.md` (run `git -C ~/Projects/tracehound-brain pull` first). **Every number, rank and claim you write comes from that file, with its wording rules (§0).** If a number you need isn't there, write `[[CHAT: what's missing]]` instead of computing or guessing it. Three slots are still open there (`[[TBD: …]]`: Ultra run 11, the seeded-PR run, levers v2); carry them as `[[TBD-ULTRA]]`, `[[TBD-SEEDED]]`, `[[TBD-LEVERS]]` so a later pass can fill them.

## Step 0 (STOP and report if a check fails)
- `git fetch origin`; `git status --porcelain` is empty.
- The `GIT_*` env check prints nothing: `env | grep -E '^GIT_(DIR|WORK_TREE|INDEX_FILE|OBJECT_DIRECTORY|ALTERNATE_OBJECT_DIRECTORIES|COMMON_DIR|NAMESPACE|PREFIX|CONFIG)'`.
- Read: `submission/numbers.md` (all of it), the brain's `prompts/2026-10-23/bunny-04-devpost-drafts.out.md` (the 12 `[[SUNNY]]` questions), both PR branches, `docs/bench/v2-calcom.md`, `docs/bench/v2-intervals.md`, `docs/bench/reviewbench.md`, `docs/bench/v3-recall.md`.

## Part A: README (#54, branch `docs/judges-readme`)
1. `git switch docs/judges-readme`, then `git merge --no-edit origin/main` (no rebase). Resolve nothing by deleting main's content; if a conflict isn't trivial, STOP.
2. Fill the placeholders from numbers.md:
   - `[[RESULTS-TABLE]]` → §1 (blind Cal.com, both judges) and §2 (all 50) with the 069b allowed sentences. Names are allowed in the README.
   - `[[CALCOM-V2]]` → one paragraph: v2 is the product config; second blind look; pre-registered; v1 row kept beside it.
   - `[[REVIEWBENCH]]` → §4: the six test runs table, "test runs, not a leaderboard submission", why there's no final (cost, measured), selection bias, and the "3.5 won on dev but lost on ReviewBench" finding.
   - `[[COST]]` → §8.
   - `[[SEEDED-TEST]]` → `[[TBD-SEEDED]]`.
   - Add a short **"Losses"** list (§6) and **"What's next"** (§9) if the README has no such section.
3. Fix what bunny-04 found stale: the install block pins `review-freeze-2` (`@0aeef06…`, not `@main`) with `model: 'nvidia/Nemotron-3.5-Super-VL'`; "Built with" says 3.5 Super is the product model (decision 068), not "being evaluated".
4. The live demo (§7): don't present the old demo reviews (Nano generators) as the current config. Say demo PR #2 posted no findings under 3.5 and link the seeded PRs as `[[TBD-SEEDED]]`.
5. The "no network" wording for the analyzer is exactly numbers.md §0 rule 10.
6. Commit (`readme: …`), push.

## Part B: Devpost drafts (#58, branch `docs/devpost-drafts`)
1. `git switch docs/devpost-drafts`, `git merge --no-edit origin/main`.
2. `writeup.md`: replace every placeholder with numbers.md content, following §0 rule 4 (**no competitor names or logos**; use "five commercial reviewers", "the leading commercial reviewer", etc.). Add the ReviewBench test runs, Ultra (report only; `[[TBD-ULTRA]]` for run 11), losses and what's next.
3. **Answer these `[[SUNNY]]` questions yourself, as chat decided:**
   - Q2 (066's named claim): use numbers.md §2's allowed sentence without names.
   - Q3 (name the AI coding assistants): yes, factually in "How we built it" (Claude Code, Codex, Cursor, Devin, Grok, Gemini), no logos.
   - Q4 (what's next): numbers.md §9.
   - Q8 (README pin): yes, the Action reference in the install block (`@0aeef06…`, tag `review-freeze-2`).
   - Q9 (demo without a judge's key): browsing recorded reviews, the dashboard and the seeded PRs is enough; say judges can't trigger a review without write access, and why.
   - Q11 (marks on screen): NVIDIA/Nebius names may appear where their own UI or our model id shows them; no logos as decoration. GitHub's UI may appear.
   - Q12: holds — `review-freeze-2` was tagged 23 Oct 18:11 IST and 069's plan pushed 13:38:03Z (19:08 IST), both before the first Cal.com 3.5 call (13:38:51Z).
   - **Leave for Sunny** (keep the `[[SUNNY: …]]` marker, list them in the report): Q1 (personal reason), Q5 (YouTube URL), Q6 and Q7 (what existed before 26 Aug / 30 Sep), Q10 (Vercel until 15 Dec).
4. `video-script.md`: update every number from numbers.md, keep it ≤ 3:00, no competitor names or logos on screen, and show the live config honestly (3.5, freeze-2). Add a shot list column "what's on screen" if missing. If a shot depends on the seeded PRs, mark it `[[TBD-SEEDED]]`.
5. `checklist.md`: update statuses and owners for 27–29 Oct (owners: Sunny, chat, relay, Bunny, Cursor, Gemini).
6. Commit (`docs(devpost): …`), push.

## Gates
Before each commit: the `GIT_*` check; `pnpm test` and `pnpm typecheck` as their own commands (the viewer test mirrors `docs/bench/results.md`, which you don't touch). Timeouts-only failures: rerun those files with `--testTimeout 30000` and note it.

## Rules
- May touch: `README.md` (on `docs/judges-readme`), `docs/devpost/*` (on `docs/devpost-drafts`). Nothing else.
- Don't touch: `docs/bench/`, `docs/decisions.md`, `packages/`, `scripts/`, `viewer/`, `.github/`, `config/`.
- Never merge a PR. Keep both drafts. No tags, no rebase, no force-push.
- Don't open Cal.com records, ReviewBench `golden/`, `linea-seeded`, `recall-seeded`, `tracehound-seeds`.

## Report (final message, and write it verbatim to `~/Projects/tracehound-brain/inbox/2026-10-27-sunny-bunny-06.md`, then `git pull --rebase`, commit, push)
Step 0 results; both PRs' head shas; every placeholder filled (and with which numbers.md section); every `[[TBD-…]]`, `[[CHAT: …]]` and `[[SUNNY: …]]` left; gate results.
