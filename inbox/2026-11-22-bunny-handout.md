# bunny handout-history (raw lane report, 2026-11-22)

- **What:** drafted sections 1–7 of Sunny's final TraceHound handout:
  1. what it is;
  2. the timeline from FixFlow to decision 081;
  3. features with status labels;
  4. techniques;
  5. challenges and dead ends;
  6. every benchmark result with its limits;
  7. costs.

  About 8357 words, in the second person, with every number linked to its source and a Sources line per section.
- **Where:** `tracehound-testset/run/out/bunny.handout-history.draft.md`, a draft that artu1 merges into
  `~/Projects/TraceHound-handout.md`. The handout is kept outside every repo: not committed to TraceHound, the test set
  or this vault.
- **Sources:**
  - brain: NOW, timeline, results, lessons, glossary, prs, project/*, history/*, ops/*, benchmark/track-b-testset,
    decisions (digest, P001/P002/P005/P010, D060);
  - TraceHound main `cfd740b`: docs/bench/results.md, ts-heldout.md, reviewbench.md, docs/eval/heldout-results.md,
    docs/budget.md, docs/devpost/{writeup,what-changed-since-26-aug}.md, README, CLAUDE.md, .agents/STATE.md,
    docs/decisions.md (076m/p/t).

  Totals only for held-out and Cal.com. No held-out folder, bench folder, ops or Recall opened.
- **Brain items flagged for artu1** (details in the draft's last section):
  - FixFlow "open question" vs the repo's answer;
  - results.md has no v2 (069) rows;
  - $19.31 vs budget.md's corrected $19.13;
  - costs.md's $45 cap vs the $61.09 hard stop;
  - a live `/pricing` page vs "no paid tier";
  - stale overview and architecture notes;
  - the roadmap's "graph view not built";
  - digest header and its missing 076j–078;
  - the runbook's Codex end date.
- **Cost:** $0, no model call.
