# Phase 2: repair agent and the held-out evaluation (decisions 022, 026–050, 30 Sep – 6 Oct)

**Question:** does giving a repair agent the graph help it fix bugs?

## What was built

- **Sandbox:** Nebius Sandboxes (Contree) were tried first (022). Support case AISTUDIOSUP-1966 said they weren't available on this account, so **local Docker became the sandbox of record** (036). It runs as uid 1000 with caps dropped, 2 GB / 2 CPU / 512 pids, network proven off after setup, no host mounts, and removes itself. A sandbox that expires or vanishes is FAILED with a reason, never UNRESOLVED (037).
- **Harness:** provider interface; verification owned by the harness, not the agent (026); repo profiles; a typecheck gate matched to a baseline (041); a per-test regression gate (043).
- **Agent:** host-side loop with native tool calls (029). Versions v2 → v6:
  - v2: environment facts, repeat guard. **Failed the toy gate** at 2/5.
  - v3: output cap, stuck stop.
  - v4: reasoning on, `/scratch`.
  - v5: graph packet injected in the first message, because the agent never called graph tools.
  - v6: `str_replace_editor` alias, edit retry.
  (030, 033, 034, 045, 047)
- **Tasks:** toy tasks; four seeded dev tasks on Recall (044).
- **Runner** (046). **Protocol** written before any held-out task existed, plus amendment 1 ("verified at stop", run order, canary per pass) (049). Freeze tag `eval-freeze-2`.
- **Reproduce stage** (048): given a claim, the agent writes one failing test, and a fresh sandbox checks it independently. Dev: Super reproduced 3 of 4 seeded claims (all true by oracle) and 1 of 4 controls; Nano 0 of 4. The frozen repair agent on Nano then resolved 1 of 3 reproduced tasks.

## The held-out result (050)

8 seeded tasks on Recall, graph on vs off, Nano and Super, 5 repeats each: 160 counted runs, $5.72 (ledger total for the window $6.27).

| Model | Resolved, on | Resolved, off | Verified at stop, on | Verified at stop, off |
|---|---|---|---|---|
| Nano | 11/40 | 13/40 | 11/40 | 19/40 |
| Super | 28/40 | 26/40 | 30/40 | 31/40 |

- **0 graph tool calls in 80 graph-on runs.**
- The model mattered far more than the graph: Super resolved 54/80, Nano 24/80.

**Verdict:** a loss for the hypothesis, reported honestly in the README. A sound call, because pre-registration made the loss believable ([D050](../decisions/D050-graph-did-not-help.md)). **Lesson carried forward:** never offer the model optional tools and expect it to use them. Inject context deterministically. 057 and 058 follow this.
