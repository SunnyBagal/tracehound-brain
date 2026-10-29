# Q6 / Q7 for docs/devpost/what-changed-since-26-aug.md (draft in Sunny's voice; Sunny confirms every fact)

Drafted by chat from what Sunny has said before (memory): the idea started as FixFlow, a Devin-style coding agent from the 100xDevs AI course syllabus; in August it became the "FixFlow V2" spec (an evidence-backed component graph for TypeScript repos with a repair agent on top); coding hadn't started by late September; the repo was created empty on 29 Sep; from 30 Sep everything was built with Claude Code from that spec.

**Facts to confirm before pasting (change the text if any is wrong):**
1. Before 26 Aug there was no code at all, only the idea and notes.
2. The V2 spec was written in August (before or after 26 Aug?). Is it public anywhere?
3. Nothing from FixFlow was carried over as code.
4. The 30 Sep commits were written that day, by Claude Code from the spec, not imported.

---

## Before 26 August  (replaces the first [[SUNNY]] block)

Before 26 August there was no code. TraceHound started as an idea called FixFlow: a Devin-style agent that takes a GitHub issue and writes the patch, which I picked from a coding-agent project in the 100xDevs AI course. In August I rewrote it as a spec, "FixFlow V2": instead of another issue-to-patch agent, an evidence-backed component graph of a TypeScript repo, with a repair agent working on top of it. That spec was a document, not code, and it isn't public. Nothing from FixFlow was carried over as code.

## The 30 September commits  (replaces the second [[SUNNY]] block)

That work was written on 30 September, not brought in from somewhere else. I created the empty repository on 29 September and built from the V2 spec with Claude Code agents, several sessions in parallel, which is how one day ended up with 76 commits. Decisions 001–031 are the spec's design choices, logged one by one as each was built.

## In one paragraph  (replaces "[[SUNNY: one sentence on what existed before 26 August.]]")

Before 26 August, TraceHound was only an idea and, from August, a written spec (FixFlow V2); there was no code.
