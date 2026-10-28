# codex-18: fill every [[TBD-SEEDED]] from numbers.md §7 (README #54, Devpost #58, video script)

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Paste everything below the line. Start after Sunny has made `SunnyBagal/tracehound-seeds` public.

---

The seeded test is scored. Fill the seeded slots. **Copy numbers and wording only from `~/Projects/tracehound-brain/submission/numbers.md` §7 (and §0's rules); add no number of your own.** Docs only, $0, no model calls. Don't open the seeded repos' code; the PR links in §7 are all you need.

## Step 0 (STOP on failure)
- `cd ~/Projects/tracehound-brain && git pull --rebase`; §7 has the seeded table and the "Allowed sentence".
- `gh repo view SunnyBagal/tracehound-seeds --json visibility` is `PUBLIC`.
- In `~/Projects/TraceHound-codex`: `git fetch origin`, clean tree. For each branch below: switch, equal to origin, `git merge origin/main` (README-only conflict on #54: keep the branch side, as before; anything else: STOP).

## 1. README, PR #54 (`docs/judges-readme`)
- "### Seeded pull requests on the live config" `[[TBD-SEEDED]]` → the allowed sentence; §7's table (class / caught / Recall / Linea, incl. the controls row); one line each: pre-registered 19 Oct (`a4a6c83`, hashes published before any PR), committed scorer with automatic rule, link `https://github.com/SunnyBagal/tracehound-seeds/blob/main/RESULTS.md`; the contract-check split (3 of 4 cross-file catches are deterministic); not run: other reviewers (names are fine in the README: CodeRabbit, Greptile, Bugbot), 3.5 not 3 Super, Recall at `0aeef06` / Linea at `86fd5d7`; the deviations list from §7, compact.
- "Live demo" `[[TBD-SEEDED]]` → "Seeded PRs on the current config: 30 on `SunnyBagal/recall-seeded` and `SunnyBagal/linea-seeded` (results above)." with both repo links.
- "Losses": add "The seeded test: 4 of 16 cross-file defects caught (0 of 4 semantic), 3 of them by the deterministic contract check."

## 2. Devpost, PR #58 (`docs/devpost-drafts`) — **no competitor or tool names anywhere** (§0.4)
- `writeup.md`: each `[[TBD-SEEDED]]` → the allowed sentence (or, in the "Trying it without a key" line, "the 30 seeded PRs on two public mirror repos" with both links). Remove the open-slot bullet for TBD-SEEDED at the top. "Not run" is written as "no other reviewer was run on these PRs".
- `checklist.md`: TBD-SEEDED rows → done (seed-01, 28 Oct); rows 7–8 updated.
- `video-script.md`: shot 2 → the model catch (§7's `recall-seeded/pull/4` link: a check-then-act race, line comment), narration says the reviewer is Nemotron 3.5 Super. Add the contract-check catch (`recall-seeded/pull/7`) as a short beat, **narrated as "the deterministic contract check, no model"**. Shot 6 or the closing card gets the allowed sentence. Keep the script ≤ 3:00 (adjust timings; cut words elsewhere, not the honesty). Shot 3: if the dashboard can't show recall-seeded yet, show the GitHub review instead and note "[[UI: recall-seeded on /reviews if #65 has the flag on]]" for Sunny.
- Leave every `[[SUNNY…]]` untouched.

## Checks (report each)
- `grep -c "TBD-SEEDED"` = 0 in README.md, writeup.md, checklist.md, video-script.md.
- `git grep -n -i -E "cursor|augment|greptile|coderabbit|copilot|bugbot"` on #58's writeup.md and video-script.md prints nothing.
- Every number in your additions appears in numbers.md §7 (list them).
- No "beats/best/outperforms/state of the art".
- Push both branches; don't mark ready.

## Report (final message)
Head shas, each replaced slot (old → new), the check results. Also to `~/Projects/tracehound-brain/inbox/2026-10-28-codex-18.md` (pull --rebase, add only that file, commit, push).
