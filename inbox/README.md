# Inbox

Quick notes from Sunny, and **raw reports from builder lanes** (Bunny, Sunny's Claude Code). The chat files each one into the right note, checks it, and deletes it here.

## Rules for builders (Bunny / Sunny lanes)
- Write **only** in `inbox/`. One file per job: `inbox/<YYYY-MM-DD>-<lane>-<job>.md`, for example `inbox/2026-10-20-bunny-066.md`.
- The content is your **final report, verbatim**: PR number, head sha, one line per commit, test counts, and anything you couldn't do. Nothing else.
- **Never edit** `NOW.md`, `prs.md`, `results.md`, `timeline.md`, `lessons.md`, `decisions/`, `research/` or `prompts/`. The chat checks the PR and updates those; a builder never marks its own work as checked.
- The brain is **public**: no secrets, no `.env` values, no Cal.com review-comment text, nothing from the seeded repos or ReviewBench `golden/`.
- To push: `git pull --rebase`, add only your inbox file, commit (`inbox: <lane> <job> report`), `git push`. If the push is rejected twice, stop and say so in your reply.
