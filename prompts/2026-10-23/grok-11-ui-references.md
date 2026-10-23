# grok-11: UI references for the TraceHound frontend redesign

Tool: **Grok bots**. Paste each block into the named bot. Same rules as grok-09/10: plain-text URLs with page dates where shown, UNVERIFIED if not opened, quotes of 15 words or fewer. Send final answers to the planning chat.

Context for the bots (paste with the Researcher block): TraceHound is an open-source AI pull-request reviewer (a GitHub Action on NVIDIA Nemotron via Nebius Token Factory). Its website (https://tracehound-tau.vercel.app) has three pages: a **component graph** of a repo, **Reviews** (findings and suggested fixes from its GitHub reviews), and **Benchmark** (tables against other reviewers). It's being redesigned in the next 3 days for a hackathon video and judges. This is design inspiration only: no copying of logos, brand assets or text.

## Job F · Researcher, then Verifier: what good looks like
**Researcher:**
> Find **10 references** of how the best developer tools present themselves. Open each page yourself. Cover these four groups:
> 1. **AI code-review / dev-agent landing pages** (3–4): how the hero explains "what it does" in one line, how they show a real review comment, how they show install (a YAML or one command).
> 2. **Benchmark / leaderboard pages for AI dev tools** (2–3), e.g. a public leaderboard or a vendor's benchmark page: how they show methodology, uncertainty (intervals, n), and "where we lose" honestly.
> 3. **Code / graph visualisations in dev tools** (2): a dependency graph, a service map, a call-graph UI that reads well.
> 4. **Recent hackathon winners with strong UI** (1–2), ideally Devpost dev-tool winners from 2025–2026.
>
> For each, give:
> - name and URL, and the date the page shows if any;
> - what works, in 2 lines;
> - **one concrete pattern to borrow**, e.g. "sticky left nav with section anchors" or "comment card shows file:line, severity chip, one-line fix";
> - one thing to avoid.
>
> Also answer:
> - (a) the 3 patterns that show up in most of them;
> - (b) how they make a dark theme readable for code and tables.
>
> Then send everything to the Verifier.

**Verifier:**
> Open every URL yourself. For each: does the page exist, does the described pattern actually appear on it (yes / partly / no), and the page date. Drop any reference you can't open. Return a table, and the 3 strongest patterns with the references that support each.

## Planner: no change.
