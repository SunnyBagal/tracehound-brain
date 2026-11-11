# Inbox

Quick notes from Sunny, and **raw reports from agent lanes** (one file per job, verbatim). `inbox/` stays for raw
agent reports only; curated notes live in their own folders.

## Rules for agents (Sunny's ruling, 2026-11-11)
- **Agents may edit curated notes** (`decisions/`, `NOW.md`, `timeline.md`, `lessons.md`, `decisions/_index.md`,
  `decisions/digest.md`, and the other maps) when a brief asks them to, following `README.md` (format, verdict
  fields, "link, don't copy", stated vs judged).
- **Every brain change needs a codex GO before it is pushed.** The agent leaves its change uncommitted (or on a branch);
  codex reviews the diff (numbers against their TraceHound sources, nothing private or held-out); artu1 or the chat
  commits and pushes after GO.
- Raw reports: `inbox/<YYYY-MM-DD>-<lane>-<job>.md`, for example `inbox/2026-10-20-bunny-066.md`, with the final report
  verbatim (PR number, head sha, one line per commit, test counts, anything not done). The chat files them and deletes
  them here.
- An agent never marks its own work as checked.
- The brain is **public**: no secrets, no `.env` values, no Cal.com review-comment text, nothing from the seeded repos or
  ReviewBench `golden/`, nothing held-out, and no tracehound-testset case details (case IDs, case PRs, labels): counts
  and method only.
- To push (after GO): `git pull --rebase`, add the reviewed files, commit, `git push`. If the push is rejected twice, stop
  and say so.
