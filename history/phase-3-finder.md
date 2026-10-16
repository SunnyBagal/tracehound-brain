# Phase 3: the finder (decisions 051–053, 6–14 Oct)

**Idea:** if the graph doesn't help repair, maybe it can choose *what to check*. Graph rules emit bug hypotheses, which become claims for the reproduce stage.

- **Phase 1** (051, `finder-rules@1`, no model): deterministic probes read source only at graph evidence. Rule families: `route-without-auth`, `payload-field-missing`, `request-to-fetch`. On Recall @ `5d2165a` they produced 1 / 0 / 2 hypotheses.
- **Phase 2a** (052, no model): hypotheses become Form A claims (question plus input) and Form B claims (plus excerpts). Recall `57d920e`: 1 / 0 / 1 hypotheses, 4 claim files. **The reproduce stage never ran on them.**
- **Disclosure** (053): the rule families were chosen by a chat that knew Recall's real bugs, so no finder hit on Recall counts as blind. Results on those bugs are reported only as "N of 3", with a pointer to 053.

**Verdict:** stopped by the pivot on 7 Oct, before any end-to-end result. Rules find only rule-shaped bugs, and the blindness problem made Recall useless as a headline. **Call:** unclear. It was reasonable to try, but it repeated the phase-1 risk of tuning on a seen repo.

**What survived:** the reproduce stage idea (the planned "proven" tier, now cut), and the blindness discipline that shaped the reviewer benchmark.
