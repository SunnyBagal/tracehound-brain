# grok-10: next Grok bot jobs

Tool: **Grok bots**. Paste each block into the named bot. Same rules as grok-09 (no `golden/`, no seeded repos, plain-text URLs with page dates, UNVERIFIED if not opened). Send final answers to the planning chat.

## Job D · Researcher, then Verifier: benchmark clauses in the seeded-test tools' terms
**Researcher:**
> For each of **Greptile**, **Cursor Bugbot** and **CodeRabbit**, open the current Terms of Service (and any acceptable-use or API terms they link). Answer: (1) Is there a clause about publishing benchmark or performance results? Quote it (15 words or fewer) with the section number and the page's "last updated" date. (2) Any clause against automated or bulk use of free or trial plans? Quote it. Write "not found" if none. Then send everything to the Verifier.

**Verifier:**
> Open every URL yourself. For each quote: confirmed / wrong / not on the page, and the page's "last updated" date. Return a table.

## Job E · Researcher, then Verifier: the hackathon submission checklist
**Researcher:**
> For the **Nebius × NVIDIA Global AI Hackathon** on Devpost (deadline 30 Oct 2026, 22:30 IST), open the official rules and submission pages. List every **required** submission item:
> - video (maximum length, where it's hosted, whether it must be public);
> - repo (public? license?);
> - write-up fields;
> - the required use of Nebius or NVIDIA (e.g. Token Factory, Nemotron);
> - a live demo link;
> - the team/individual form;
> - anything about using AI coding tools or pre-existing code.
>
> Also list the **judging criteria with weights** for the "Coding and Agentic Engineering" track. Quote each item (15 words or fewer) with its URL. Then send everything to the Verifier.

**Verifier:**
> Open every URL yourself. Confirm or correct each item. Flag anything that would disqualify a submission. Return a checklist table.

## Planner: no change (daily digest at 9:00 IST).
