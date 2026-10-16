# TraceHound brain

The project's memory outside any one chat: what was built, what was tried, what the numbers were, why each call was made, and whether it was right. The repo (`SunnyBagal/TraceHound`) is the source of truth for code, results docs and the decision log. This vault is the map to all of it, plus what the repo doesn't hold: current state across lanes, hindsight verdicts, process lessons, chat decisions, and the comparison with other tools.

## Start here

1. **[NOW](NOW.md):** current state, in-flight jobs, the next steps. Read this first.
2. **[Overview](project/overview.md):** what TraceHound is, in one page.
3. **[Results](results.md)** and **[strengths and gaps](strengths-and-gaps.md):** where we stand.
4. **[Lessons](lessons.md):** mistakes not to repeat.

## Map

| Question | Note |
|---|---|
| What is it, what's live? | [project/overview](project/overview.md) |
| Where is the code, which folder is whose? | [project/architecture](project/architecture.md) |
| What does the hackathon require, what's missing? | [project/hackathon](project/hackathon.md) |
| What's planned, what got cut? | [project/roadmap](project/roadmap.md) |
| What happened, in order? | [timeline](timeline.md), [prs](prs.md) |
| What did each phase try and learn? | [history/phase-1-graph](history/phase-1-graph.md), [phase-2-repair-eval](history/phase-2-repair-eval.md), [phase-3-finder](history/phase-3-finder.md), [phase-4-reviewer](history/phase-4-reviewer.md) |
| Every decision ever made, searchable | [decisions/digest](decisions/digest.md) (all repo decisions), [decisions/_index](decisions/_index.md) (with vault notes) |
| Was a decision right? | `decisions/Dnnn-*.md` (hindsight on repo decisions), `decisions/Pnnn-*.md` (chat decisions); table in `decisions.base` |
| How is the benchmark scored, and what are its weaknesses? | [benchmark/benchmark](benchmark/benchmark.md) |
| What do CodeRabbit, Greptile, Augment, Cursor and Copilot score? | [benchmark/competitors](benchmark/competitors.md) |
| What are the numbers? | [results](results.md) |
| How do I start relay, check a PR? | [ops/runbook](ops/runbook.md) |
| How much have we spent? | [ops/costs](ops/costs.md) |
| What does a term mean? | [glossary](glossary.md) |

## Who reads and writes

| Who | Reads | Writes |
|---|---|---|
| New Claude chat | NOW, lessons, then whatever NOW points to | Everything, after checking each PR ([runbook](ops/runbook.md), "How the chat checks a PR") |
| Relay | Only notes a brief names, by `~/Projects/tracehound-brain/...` path | Nothing |
| Codex | Only notes a task names (overview, results and competitors feed the judges' README) | Nothing |
| Grok | Raw GitHub URLs it's given | Nothing; the chat records its findings |
| Sunny | Anything | Anything; his verdict overrides the chat's |

## Rules (non-negotiable)

- **Public repo.** Nothing goes in that can't be posted.
- **Blindness.** No Cal.com content: no golden comment text, no tool comment text, no per-comment detail. **No TraceHound Cal.com number until the results doc is published.** Dataset sizes and the tools' published Cal.com counts may be linked, not copied. No description of Recall's real bugs (repo decision 053).
- **No secrets**, keys or personal data.
- **Link, don't copy.** Restate only the numbers a verdict needs; the repo doc is the source.
- **Stated vs judged.** "Expected" records only what was written down before a result. Verdicts are hindsight, with who judged and when.

## Verdict fields (decision notes)

- `outcome`: what happened. `win`, `loss`, `tie`, `pending`, `n/a`.
- `call`: was the decision reasonable given what was known then? `sound`, `flawed`, `unclear`, `too-early`. `flawed` needs a "What we'd do differently" line naming the mistake.

## Upkeep (the chat does this after every PR check)

NOW → prs → results → timeline → the decision note (outcome and call) → strengths and gaps → lessons (if a process mistake happened) → regenerate the digest when a decision lands. If a chat reads a note that disagrees with GitHub, GitHub wins: fix the note.
