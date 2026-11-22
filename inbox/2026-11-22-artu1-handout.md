# artu1 handout: final project handout for Sunny (raw)

Sunny's last task before closing the sessions, 22 Nov: one Markdown handout to understand TraceHound as a whole and
explain it in interviews. It is kept outside both repos (Sunny decides later whether any of it goes public). $0, no
model call.

- **Lanes:** bunny wrote sections 1–7 (story, timeline, features with status labels, techniques, challenges, results,
  costs) from the brain; artu1 wrote 8–10 (architecture from the code on TraceHound main `cfd740b`, glossary, 15
  interview questions) and merged them; grok fact-checked the merge (`2026-11-22-grok-handout-factcheck.md`).
- **Rules kept:** every number linked to a published page or a brain page; a Sources line per section; held-out totals
  only; Cal.com aggregates only; no secrets; nothing in the bench, ops, eval, heldout or Recall folders opened.
- **Disagreements found and listed in the handout's appendix** (none silently resolved): FixFlow carry-over; the
  brain's results page stops at Cal.com v1 while the published page is v2; the 9 Nov balance ($19.31 vs $19.13); the
  $45 cap vs the $61.09 hard stop; a live `/pricing` page with paid plans while band D says no paid tier; stale
  overview and architecture notes; the per-PR graph view; the digest's gaps (076j–078); codex's end date; two analyzer
  version numbers (package.json 0.6.0 vs ANALYZER_VERSION 0.10.0); two freeze tags in use (2.1 on the workflow, 3 for
  the held-out run).
- **Fact-check:** grok NO-GO with 12 fixes (exact p-values 0.0078125 / 0.109375 and the page's wording instead of
  "statistically clear"; S1 pairing 4/179 = 2.2% micro, 2.1% macro; judge-x1 interval for CodeRabbit; exact money
  figures and sources; NVIDIA API is a second provider for one model, not a fallback; model input built, provider
  input never built; fresh retry still in code, not default; verify-v1 never adopted; side-track branch never merged;
  two paths; ledger vs console). All applied. Recheck: one more fix (Sonnet is a dev row only; Haiku alone on the
  held-out row), applied. Brain diff GO from grok (`run/out/grok.handout-recheck.md`).
