# codex-08: deterministic pre-filter and post-gate (S1 + S6)

Tool: **ChatGPT Codex**. Folder: `~/Projects/TraceHound-codex`. Start a **new task**. Paste everything below the line.

---

You are building two deterministic stages for TraceHound's reviewer: a diff pre-filter and a post-gate on review findings. They make **no model calls** and use no network. Design: `tracehound-brain/project/architecture-v2.md` §2 (S1, S6). You don't need it; this brief is complete.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` prints nothing.
- Create branch `codex/gate-v1` from `origin/main`, which must contain `2af8c3a` (the #43 merge).
- `pnpm test` and `pnpm typecheck` are green before you change anything.

## What to build
Make one commit per item, each with tests, and keep `pnpm test` and `pnpm typecheck` green. Stage files by explicit path.

1. **`packages/analyzer/src/gate/prefilter.ts`:** `prefilterDiff(files, options)`. It takes `DiffFile[]` from `review/diff.ts` `parseDiff` (reuse its types and `newSideLines`; don't change that file) and returns `{ kept, dropped: { path, rule }[] }`. Rules, each with a stable id:
   - `lockfile`: `pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, `bun.lock(b)`, `Cargo.lock`, `poetry.lock`, `Gemfile.lock`, `go.sum`, `composer.lock`.
   - `generated`: a `@generated` or `DO NOT EDIT` marker in the first 5 lines of the added text; paths under `dist/`, `build/`, `out/`, `.next/`, `coverage/`, `vendor/`, `node_modules/`; `*.min.js`, `*.min.css`, `*.map`; `*.pb.go`, `*_pb2.py`.
   - `snapshot`: `__snapshots__/`, `*.snap`.
   - `binary`: diffs git marks as binary.
   - `whitespace`: hunks whose added and removed lines are equal after stripping all whitespace. Drop the hunk, not the file.
   - `config-ignore`: globs from `tracehound.json` `review.ignore`. Add this optional field to `config.ts` with a default of `[]`, and keep every other config default unchanged.

2. **`packages/analyzer/src/gate/gate.ts`:** `gateFindings(candidates, ctx)`. It returns `{ kept, dropped: { finding, rule, detail }[] }`. A candidate is `{ path, line: number|null, body, source?, votes?, evidence?: {path,line,quote}[], scenario?, severity?, category? }`; declare this type locally in `gate/`. `ctx` holds the parsed diff, a `readFileAt(path) => string | undefined` reader for the head checkout, the suppressions list (an empty array for now; codex-09 fills it) and the options. Rules run in this order, and each drop records its rule id:
   - `prefiltered`: the path was dropped by the pre-filter.
   - `not-in-diff`: the line is not an added or context line in the diff. Exempt `source: "contract" | "tsc"`; those findings are about files the PR didn't edit, but their path must still exist at the head.
   - `evidence-missing`: if `options.requireEvidence` is set, the candidate has no evidence.
   - `evidence-mismatch`: a quote isn't found in the cited file within ±3 lines of its line. Compare after trimming whitespace. One mismatch drops the finding.
   - `style`: `category === "style"`, or the body matches the nit patterns. The patterns live in `gate/nits.ts` as a reviewed list, each with a fixture test. Cover naming, formatting, typos, comments or docs, readability, "consider", "nit", import order, magic numbers and unused-import-only claims. **Write this list from general knowledge. You must not look at any benchmark data, and you have no access to it.**
   - `suppressed`: matches a suppression fingerprint (the interface only; tests use a stub).
   - `duplicate`: same normalized path, lines within 3 (reuse `scripts/bench/agree-rule.ts` `agrees`; import it, don't copy it). Keep the one with more votes, then the higher severity, then the earliest.
   - `severity-floor`: below `options.minSeverity`, if set. Candidates without a severity always pass this rule.
   - `cap`: keep at most `options.maxPerPr`, ranked by votes, then severity, then order.
   With all options unset, only `prefiltered`, `not-in-diff`, `evidence-mismatch` (only when evidence is present), `style` and `duplicate` apply.

3. **CLI:** `tracehound gate --pr <input.json> --findings <file.json> --repo <head checkout> --out <file.json> [--require-evidence] [--min-severity medium] [--max-per-pr N]`. The output is `{ kept, dropped, counts: { [rule]: n } }`. Add one line to `bin.ts`. The CLI shape has to work on the recorded dev outputs that the relay lane holds, which are `findings: [{path,line,body}]` per PR.

4. **Tests:** `packages/analyzer/test/gate.test.ts` (the tests in this package are flat files), with fixtures under `packages/analyzer/test/fixtures/gate/`. Every rule needs a positive case and a negative case. Add a property-style test: running the gate twice is the same as running it once (idempotence). Add a test that pool-verify-v1's outputs pass through unchanged when no rule fires.

5. **Docs:** `docs/gate.md` gives one line per rule: id, what it drops, and why.

## Rules
- Touch only: `packages/analyzer/src/gate/`, `packages/analyzer/test/gate.test.ts`, `packages/analyzer/test/fixtures/gate/`, one line in `packages/analyzer/src/bin.ts`, the new optional `review.ignore` field in `packages/analyzer/src/config.ts`, and `docs/gate.md`.
- **Don't change** anything else under `packages/analyzer/src/review/`, `packages/action/` or `scripts/bench/`, nor any prompt or `config/`. pool-verify-v1 must stay byte-identical; the Action wiring is a later job.
- No new dependencies. No Nebius or network calls. No force-push, no merge.
- Open the PR to main **as a draft** titled `gate-v1: deterministic pre-filter and post-gate (S1, S6)`. Wait for CI and make one fix attempt if it's red.

End with: the PR number, the head sha, one line per commit, the test count, and a table of rule id → test cases.
