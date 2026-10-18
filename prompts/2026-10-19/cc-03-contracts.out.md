# cc-03 output (Claude cloud, pasted by Sunny 19 Oct 03:22). Checked by chat: see NOW / prs.

Node 24 isn't installed (only 20–22). Let me check whether nvm can fetch it, then run the baseline gates.

Step 0 passes: branch from `origin/main` (contains `2af8c3a`), install, tests (action 153, analyzer 496 + 7 skipped, viewer 186), typecheck all green on Node 24.22. Now reading the existing code.

Step 0 is green; I'm reading the extractors to design the keyed stage before writing `contracts/typed.ts`.

Design settled. Writing the shared worktree helper and `contracts/typed.ts` first.

I've written `trees.ts` and `typed.ts`. Now I'm writing the fixture cases (7 TS break/control pairs, plus Python and Go).

Now the typed tests.

Typed stage: 26 tests pass. Running typecheck, then committing item 1.

Item 2 next: `contracts/shapes.ts` (key-like literal shapes) and `contracts/keyed.ts`.

Keyed stage written with its tests; running them now.

All 54 keyed tests pass. Typechecking.

Item 3: `contracts/dependents.ts`, the dependents of changed exported declarations (data only, no findings).

Dependents stage passes its 8 tests; committing item 3, then on to findings.

Item 4: `contracts/findings.ts`, the deterministic candidates.

Item 5: the pipeline, CLI and the `bin.ts` line.

Item 6: an end-to-end fixture test through `computeContracts`, plus an event-name pair so every key kind has a positive case and a control.

Fixture table is all green (10 breaks, 10 controls). Item 7: runtime check on the real repos. Cloning them shallowly into the scratchpad.

Linea and Recall run in 2–7 s per commit. Now excalidraw.

All three repos finish inside the 120 s budget (excalidraw: typed ~51 s, total 57–72 s). Checking where typed spends its time on excalidraw before writing it up.

The real cost is legit type-checking (~9s per side building/binding 11 configs), but the extra keyed/dependents time comes from adding files one at a time, which forces ts-morph to rebuild the program between lookups. I'll fix this by pre-adding all files before resolving symbols.

`identifiersByLine` costs 8 s (wrapping every node in ts-morph). Switching to position lookups on the hit line.

Worktree grep is 8× faster than grepping the commit tree. Switching both stages to grep the head worktree.

All 9 real-repo runs completed within budget with no findings. Before writing docs, I'll do a quick sanity check by seeding a break on a scratch copy of excalidraw to confirm the typed stage catches it without node_modules installed.

All 31 real call sites of the changed function were flagged as TS2554, and the one unflagged grep match turned out to be a string in a describe block, not an actual call. Now writing docs/contracts.md (item 8).

All green: analyzer 637 passed / 7 skipped (141 new contracts tests), action 153, viewer 186, typecheck clean. Committing docs, checking the touched paths, then pushing.

I've built the cross-file contracts stage and opened it as a draft PR: [SunnyBagal/TraceHound#47](https://github.com/SunnyBagal/TraceHound/pull/47). Every fixture break is caught and every control gives zero findings. On 9 real commits it produced no findings and finished well inside the 120 s budget.

**PR:** [SunnyBagal/TraceHound#47](https://github.com/SunnyBagal/TraceHound/pull/47), draft, branch `cc/contracts-v1` → `main`
**Head sha:** `d469e287b073937eb094be92218bdd97c5211881`

**Step 0:** passed. The branch is from `origin/main`, which contains `2af8c3a`; install, tests and typecheck were green before any change. The container only had Node 20–22, so I installed Node 24.22 to match the repo's `engines`. The gate check `env | grep ^GIT_…` matches this session's proxy settings (`GIT_CONFIG_COUNT`, `GIT_CONFIG_KEY_*`, `GIT_CONFIG_VALUE_*`), so I unset those before every gate.

**Commits**
- `0ee7f9c` typed stage: tsc diagnostics new at head, with the worktree helper and the fixture cases
- `4fec95f` keyed stage: changed string keys and their stale uses (`shapes.ts`, `keyed.ts`)
- `557cc19` dependents of changed exported declarations
- `6522bd3` deterministic findings from the typed and keyed stages
- `33d3ad6` `tracehound contracts` CLI, plus one line in `bin.ts`
- `baa4383` end-to-end fixture tests, plus an event-name pair so every key kind has a positive and a control
- `686d473` speed-ups found on excalidraw: grep the head worktree (8× faster per search), add all files before resolving symbols, look up identifiers by position
- `d469e28` `docs/contracts.md`

**Tests:** 141 new contracts tests.
- `pnpm test`: analyzer 637 passed, 7 skipped; action 153 passed; viewer 186 passed.
- `pnpm typecheck`: clean.

**Fixtures** (expected = got for every row)

| Fixture | Expected and got |
|---|---|
| required parameter / optional | tsc 1 (`src/app.ts:2`) / 0 |
| field removed / added | tsc 1 (`src/view.ts:2`) / 0 |
| export renamed, stale importer / renamed everywhere | tsc 1 (`src/page.ts:1`) / 0 |
| nullable return, caller dereferences / caller guards | tsc 1 (`src/caller.ts:2`) / 0 |
| queue renamed in the producer only / both sides | contract 1 (`src/worker.ts:2`) / 0 |
| env var renamed in config only / all readers | contract 1 (`src/db.ts:1`) / 0 |
| route changed on the server only / client updated | contract 1 (`src/client.ts:2`) / 0 |
| event renamed in the emitter only / both (added) | contract 1 (`src/listener.ts:2`) / 0 |
| Python route + task / control | contract 2 (`client.py:5`, `:8`) / 0 |
| Go env + Redis key / control | contract 2 (`main.go:9`, `:10`) / 0 |

**Runtime** (4 CPUs, no `node_modules`, `HEAD~1..HEAD` per commit)

| Repo | Commits | Findings | Total time | typed part |
|---|---|---|---|---|
| Linea | `4f075a5`, `868f3f8`, `ca8b1b1` | 0, 0, 0 | 5.6 / 7.6 / 6.2 s | ~4.5 s |
| Recall | `20ac149`, `3e39129`, `9113ced` | 0, 0, 0 | 3.3 / 2.6 / 2.3 s | ~2.1 s |
| excalidraw | `e1b7aac`, `4c37956`, `541653c` | 0, 0, 0 | 64.9 / 52.9 / 57.1 s | ~48 s |

These are merged commits, so zero findings is the expected result; nine runs say nothing about how many real breaks it catches. As a sanity check I also seeded one break in a scratch copy of excalidraw (never pushed): an exported function got a required parameter. That produced 31 TS2554 findings, one for each of its 31 call sites in unedited files, and nothing else.

**Where I went beyond or interpreted the spec**
- **`side` on evidence:** each evidence item has a `side` (`base` | `head`) next to `{path, line, quote}`, because a removed line's number refers to the base revision.
- **Extra `refs` field:** the report has `refs` (base, head, merge base) alongside the five keys you listed.
- **Env vars need a rename:** an env var that is removed without a rename is listed with a note but not searched, since dropping a read isn't a break by itself.
- **Known gaps** (written up in `docs/contracts.md`):
  - Without the repo's own dependencies installed, every imported package type is `any`, so a break that only reaches an unedited file through a package's types is missed.
  - Route paths are compared without router mount prefixes.

**Not done:** none of the excluded benchmark repos was used. There were no Nebius calls, so `FEEDBACK.md` has no entry. I didn't merge anything.