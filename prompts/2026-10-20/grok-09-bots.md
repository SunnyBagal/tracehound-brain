# grok-09: first jobs for the Grok bots

Tool: **Grok bots**. Each block below goes to the bot named in its heading; paste the text under the heading. The bots need no access beyond web browsing: the brain is public at https://github.com/SunnyBagal/tracehound-brain. Send every final answer back to the planning chat, which files it and runs the link check (P003). **The bots decide nothing.**

Rules for every bot:
- Never open ReviewBench's `golden/` folder, its judge or classifier prompts, or any expected-issue file.
- Never open `SunnyBagal/linea-seeded`, `recall-seeded` or `tracehound-seeds`.
- Never quote Cal.com review-comment text.
- Write every source as a plain-text URL plus the page's date. Mark anything not opened as UNVERIFIED.

---

## Job A · Researcher, then Verifier: how the ReviewBench portal works

**Researcher:**
> Research the ReviewBench submission process (GitHub + Microsoft, launched 5 Oct 2026; blog: https://github.blog/ai-and-ml/github-copilot/reviewbench-an-open-benchmark-for-ai-code-review/). Use the public README, AGENT_CONTRACT.md, the onboarding docs and the portal's help pages only. Do NOT open the repo's `golden/` folder or any judge or classifier prompt. Answer each question with a URL and a quote of 15 words or fewer:
> 1. What are the steps from registering an agent image to approval, and how long does approval take?
> 2. Can one account register **several image digests or configurations**, or update the digest between the 25-PR test runs and the final run?
> 3. How many 25-PR test runs are allowed? Is there a limit or cooldown?
> 4. **Who pays for the judge model** in portal runs: the submitter (with their own API key) or ReviewBench?
> 5. What does a test-run report show (grounded and augmented P/R/F1, per-PR details)?
> 6. How long does a full run (219 PRs × 3 rounds) take, and what's the per-PR time limit?
> 7. Can a submitter withdraw a result or choose which run goes on the leaderboard?
>
> Then send your full answer to the Verifier bot.

**Verifier:**
> You've received the Researcher's answer about ReviewBench. Open every URL yourself; don't trust the summary. For each of the 7 answers, write: confirmed / wrong / not found on the page, plus the exact supporting quote (15 words or fewer). Do not open any `golden/` folder or judge prompt. Return a table.

## Job B · Isabella, then Verifier: X and LinkedIn post drafts

**Isabella:**
> Draft one X post (at most 280 characters, plus an optional 3-post thread) and one LinkedIn post (at most 150 words) for TraceHound, an open-source AI pull-request reviewer running on NVIDIA Nemotron via Nebius Token Factory, built for the Nebius × NVIDIA hackathon.
>
> Use **only** these numbers, worded exactly this way:
> - "On 10 blind Cal.com PRs (F1, run 1): Augment 73.2 · Cursor 66.7 · TraceHound 58.3 · Greptile 50.0 · Copilot 46.2 · CodeRabbit 36.7."
> - "Above Greptile and CodeRabbit, behind Augment and Cursor; the Greptile gap is within run-to-run noise."
> - "4th of 6 across all 50 PRs. Every loss is published."
>
> Don't say "beats", "best" or "state of the art", and invent no other numbers. Tone: honest, specific, a little dry. Link placeholder: [REPO]. Then send your drafts to the Verifier bot.

**Verifier:**
> You've received post drafts. Open https://github.com/SunnyBagal/tracehound-brain/blob/main/results.md and https://github.com/SunnyBagal/tracehound-brain/blob/main/NOW.md. For every number and every claim in the drafts, write: matches / doesn't match / not in those files, quoting the line it matches. Flag any wording that claims more than those files support. Return the drafts unchanged plus your table.

## Job C · Planner: a morning digest (daily, read-only)

**Planner:**
> Every morning at 9:00 IST, open https://github.com/SunnyBagal/tracehound-brain/blob/main/NOW.md and https://github.com/SunnyBagal/TraceHound/pulls. In at most 8 lines, report:
> - any line marked HOLD;
> - open PRs and their state;
> - what NOW says is due today or overdue;
> - any date in NOW within 48 hours (deadline: 30 Oct 2026, 22:30 IST; submit target: 29 Oct).
>
> Don't plan or suggest. Only report what the pages say, with the line you read it from.
