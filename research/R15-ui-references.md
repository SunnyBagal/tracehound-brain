# R15 · UI references for the frontend redesign (grok-11, 23 Oct 2026)

**Source:** Grok Researcher (pages opened 20:37–20:50 IST) and Verifier. Raw Researcher output: [prompts/2026-10-23/grok-11-ui-references.out-researcher.md](../prompts/2026-10-23/grok-11-ui-references.out-researcher.md). This is for inspiration only: no logos, brand assets or copy. **Competitor names and pages must not appear in the hackathon video** (R14).

## Verifier table

| # | URL | Exists | Pattern appears | Page date |
|---|---|---|---|---|
| 1 | https://www.coderabbit.ai/ | yes | yes: the comment card does flag → fix → verify | none shown |
| 2 | https://www.greptile.com/ | yes | yes: bug cards with a "See PR" link (the marquee/testimonial wasn't found) | metadata 2026-10-23 |
| 3 | https://cursor.com/bugbot | yes | yes: effect-first bug titles; it shows as a check in the merge box | none shown |
| 4 | https://www.swebench.com/ | yes | yes: task counts (2294 / 500 / 300), a "verified" icon | newest news May 2026 |
| 5 | https://aider.chat/docs/leaderboards/ | yes | yes: expandable rows with the full run record and a reproduce command; cost beside the score | last updated 20 Nov 2025 |
| 6 | https://www.greptile.com/benchmarks | yes | yes: a method statement and a per-PR case library linking real PRs | eval July 2025 |
| 7 | https://nx.dev/docs/features/explore-graph | yes | yes: focus, group, search and hide; path trace between two nodes ("proximity" control) | docs v24 |
| 8 | https://docs.datadoghq.com/tracing/services/services_map/ | yes | yes: the layout carries meaning (customer-facing on the left); health borders; inspect neighbours | metadata 2026-08-06 |
| 9 | https://devpost.com/software/project-blackbox | yes | yes: Grand Prize; a one-line pipeline told through one real incident; ✅/❌ ruled-out lines | updates Sep 2026 |
| 10 | https://devpost.com/software/blastradius (RippleProof) | yes | partly: a comparison table and stated limits; 5 captioned images, not 10 | started Aug 2026 |
| + | https://lmarena.ai/leaderboard/webdev | yes (redirects to arena.ai) | the only page showing ± intervals (e.g. "1807 +13/−13") | Oct 22, 2026 |

## The 3 strongest patterns (Verifier)
1. **Real artefacts, not mock-ups:** file paths, diffs, and links to real PRs, runs and incidents. → TraceHound: link every claim to a real PR review, run record or decision number.
2. **Honesty markers next to each claim:** n, dates, verified badges, stated limits, intervals. → TraceHound already has this. Make it visible (n, interval, "second look", "every loss published") beside each number.
3. **A summary that expands into detail:** expandable rows, open a check, narrow a graph. → benchmark rows that expand into method and per-run detail; graph focus/path trace.

## Patterns to borrow (Researcher, one per reference)
- **Comment card:** bot header, severity label, a one-line effect-first title, the diff, and a committable suggestion (1, 2, 3).
- **Install as one visible YAML block.** None of the three landing pages shows it, so it's a gap TraceHound can fill.
- **Benchmark:** a verified/second-look marker, cost beside the score, an expandable run record, and a case table linking real PRs (4, 5, 6). Show intervals: only lmarena does.
- **Graph:** don't open on the whole graph. Default to a focus view, a path trace between two nodes, and evidence on edge click (7, 8).
- **Hackathon page:** one pipeline line told through one real case; captioned screenshots; a limits section (9, 10).

## Dark theme (Researcher; values from page CSS, partly verified)
- A tinted near-black, not pure black (`#14120b`, `#111827`).
- Off-white text (`#e6edf3`–`#f3f4f6`).
- Cards and code one shade lighter (`#1f2937`, `#23262f`).
- Faint borders (`#374151`).
- No zebra rows.
