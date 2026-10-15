# TraceHound brain

The project's memory outside any one chat: what we decided, why, what happened, and whether the call was right. The repo (`SunnyBagal/TraceHound`) stays the source of truth for code, results and the decision log. This vault holds what the repo does not: the current state across lanes, hindsight verdicts on decisions, process lessons, and decisions made in chat.

## Start here

| If you are | Read | Write |
|---|---|---|
| A new Claude chat (claude.ai) | [NOW](NOW.md), [lessons](lessons.md), then whatever NOW points to | NOW, decision notes, lessons, timeline (after checking each PR) |
| Relay (Claude Code, builder or reviewer) | Only the notes your brief names, by absolute path `~/Projects/tracehound-brain/...` | Nothing |
| Codex | Only the notes your task names, by absolute path | Nothing |
| Grok | The raw GitHub URLs you are given | Nothing; the chat records your findings |
| Sunny | Anything | Anything; your verdict overrides the chat's |

Relay and Codex do not load the vault automatically. A growing set of notes in every builder session costs tokens and invites work outside the brief. Briefs name the notes that matter.

## Rules (all non-negotiable)

- **Public repo.** Nothing goes in that cannot be posted.
- **Blindness.** No Cal.com content and no Cal.com numbers of any kind until the final run is published. No golden comment text, no review comment text, no per-comment detail. No description of Recall's bugs (repo decision 053).
- **No secrets**, no API keys, no personal data.
- **Don't copy the repo.** Link to `docs/decisions.md` and `docs/bench/*.md`; restate only the numbers a verdict needs.
- **Stated vs judged.** "Expected" records only what was written down before the result. Verdicts are hindsight and say who judged and when.

## Layout

- [NOW.md](NOW.md): current state: main, in-flight jobs, money, schedule, open decisions. Replaces the handoff doc.
- [timeline.md](timeline.md): dated one-liners. Feeds the Devpost "what changed" summary.
- [results.md](results.md): dev-set scoreboard, every reviewer version.
- [lessons.md](lessons.md): process mistakes and what changed because of them.
- [decisions/_index.md](decisions/_index.md): all 57 repo decisions (linked) plus vault notes.
- `decisions/Dnnn-*.md`: hindsight notes on repo decisions. `decisions/Pnnn-*.md`: decisions made in chat, which have no repo entry.
- [templates/decision.md](templates/decision.md): the note shape.
- `decisions.base`: Obsidian table of every decision note by outcome and call.
- `inbox/`: Sunny's quick notes; the chat files them.

## Verdict fields

Each decision note has two separate judgments, because a good call can have a bad outcome and the reverse:

- `outcome`: what happened. `win`, `loss`, `tie`, `pending`, `n/a`.
- `call`: was the decision reasonable given what was known at the time? `sound`, `flawed`, `unclear`, `too-early`.

`flawed` needs a "What we'd do differently" line that names the specific mistake.
