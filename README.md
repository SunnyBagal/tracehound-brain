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
| What is the reviewer v2 design? | [project/architecture-v2](project/architecture-v2.md), [P007](decisions/P007-v2-architecture.md) |
| What exactly was each agent told, and what did it say? | `prompts/<date>/<job>.md`, its pasted output in `<job>.out.md` |
| What happened, in order? | [timeline](timeline.md), [prs](prs.md) |
| What did each phase try and learn? | [history/phase-1-graph](history/phase-1-graph.md), [phase-2-repair-eval](history/phase-2-repair-eval.md), [phase-3-finder](history/phase-3-finder.md), [phase-4-reviewer](history/phase-4-reviewer.md) |
| Every decision ever made, searchable | [decisions/digest](decisions/digest.md) (all repo decisions), [decisions/_index](decisions/_index.md) (with vault notes) |
| Was a decision right? | `decisions/Dnnn-*.md` (hindsight on repo decisions), `decisions/Pnnn-*.md` (chat decisions); table in `decisions.base` |
| How is the benchmark scored, and what are its weaknesses? | [benchmark/benchmark](benchmark/benchmark.md) |
| What do CodeRabbit, Greptile, Augment, Cursor and Copilot score? | [benchmark/competitors](benchmark/competitors.md) |
| What are the numbers? | [results](results.md) |
| How do I start relay, check a PR? | [ops/runbook](ops/runbook.md) |
| How much have we spent? | [ops/costs](ops/costs.md); why the ledger and the console disagree: [ops/ledger-vs-billing](ops/ledger-vs-billing.md) |
| Why do reviewer chunks fail, and what was tried? | [project/empty-replies](project/empty-replies.md), [project/300s-transport-cutoff](project/300s-transport-cutoff.md) |
| When may a test replay the response cache? | [benchmark/cache-survivorship](benchmark/cache-survivorship.md) |
| What is Track B, the fresh TypeScript test set? | [benchmark/track-b-testset](benchmark/track-b-testset.md) (method and counts only) |
| How do I run TraceHound locally on a test case? | [ops/local-runner](ops/local-runner.md) |
| How does the multi-agent team work (Herdr, lanes, `.done` files)? | [P010](decisions/P010-herdr-team.md) |
| What does a term mean? | [glossary](glossary.md) |
| What did outside research find? | [research](research/README.md); the 7 Nov multi-agent review: [R17](research/R17-multi-agent-review-2026-11-07.md) |

## Who reads and writes

| Who | Reads | Writes |
|---|---|---|
| New Claude chat | NOW, lessons, then whatever NOW points to | Everything, after checking each PR ([runbook](ops/runbook.md), "How the chat checks a PR") |
| Relay | Only notes a brief names, by `~/Projects/tracehound-brain/...` path | Nothing |
| Codex | Only notes a task names (overview, results and competitors feed the judges' README); the whole diff when reviewing a brain change | Curated notes only when a brief asks (then as the agents row, reviewed by another codex pass or the chat, never self-approved); gives GO/NO-GO on every brain change before push |
| Agents (bunny, grok, …) | Notes a brief names | Curated notes when a brief asks, plus raw reports in `inbox/`; uncommitted until codex GO ([inbox rules](inbox/README.md)) |
| Grok | Raw GitHub URLs it's given, and vault files a brief names | Nothing unless a brief asks (then as the agents row); otherwise the chat records its findings |
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

## Filing rule (every file the chat writes or receives)

| File | Goes to |
|---|---|
| A prompt for any lane | `prompts/<date>/<job>.md`, pushed **before** Sunny pastes it |
| An agent's pasted output (a chat-relayed prompt's answer) | `prompts/<date>/<job>.out.md` (verbatim, with a one-line header) |
| A builder lane's raw job report (written by the agent itself) | `inbox/<YYYY-MM-DD>-<lane>-<job>.md` ([inbox rules](inbox/README.md)); the chat files and deletes it |
| An agent's edit to a curated note | the note itself, uncommitted until codex GO ([inbox rules](inbox/README.md)) |
| Grok or cloud research | `research/Rn-*.md` with the chat's kill-test note on top |
| A plan or design | `project/` + a `Pnnn` note if it decides something |
| A handoff | `history/` |
| Never | Cal.com comment text or per-comment detail, secrets, raw bench data, logs, screenshots |

## Upkeep (the chat does this after every PR check)

NOW → prs → results → timeline → the decision note (outcome and call) → strengths and gaps → lessons (if a process mistake happened) → regenerate the digest when a decision lands. If a chat reads a note that disagrees with GitHub, GitHub wins: fix the note.
