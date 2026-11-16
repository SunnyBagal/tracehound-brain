# devin (Jake): pre-F1 methodology review, finish

- 07:27–07:37Z. Brief: testset `run/briefs/devin.pre-f1-methodology.md` (artu1). Read-only, $0, 0 model
  calls, tuning-side documents only. Sources: TraceHound main `955f0e6`, Planner A branch `pa-076g` `2a8eb7a`, public
  Nebius deprecation pages. Report: testset `run/out/devin.pre-f1-methodology.md` + `.done`.
- 8 findings: 3 block F1, 3 fix before the write-up, 2 notes (with sub-items).
- Blocks F1: (1) the second judge feeds the hand-check candidate set, and so the headline; if its model is gone, the kit
  reports "x1 caught 0" with no failure count, and a models-list read is not proof a model is served; (2) the second
  judge's model is past its listed removal date, and the protocol has no fallback; recommended: a pre-registered swap to
  the provider's named replacement, measured on dev before E1's remaining second-judge calls, with "every comment on the
  sheet" as the written fallback; (4) the ledger runs ~17% under the console on real calls, so the 1.08 console stop will
  trip on any clean job and the pre-tag ratio condition cannot be met by repricing; the hard stop no longer covers the
  balance; completion is unaffected (caps and costs are in the same ledger units).
- Note: the primary judge's model has an official removal date before the end of the year; H2 must finish before then.
- Fix before the write-up: the "drafted before results" disclosure has no dated evidence; the secondary-row product rule
  is a coin flip near its margin (an estimate) and must not be worded as equivalence; README/cost/four-tests/"fixed
  before any dev result" wording.
- Premise failure reported: the "three options" the brief cites are not in the LOG; four options were set out instead.
- Nothing edited outside the report, claim, done and these two notes. Uncommitted; artu1 scans and pushes.
