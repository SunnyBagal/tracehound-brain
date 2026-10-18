# cc-03: contract map and deterministic cross-file findings (S2b + S3 contracts)

Tool: **Claude cloud** (claude.ai/code), a **new session** on repo `SunnyBagal/TraceHound`. It spends the cloud credit, not Nebius. Paste everything below the line.

---

You're building TraceHound's cross-file stage. It finds what a PR changed that **files the PR didn't edit** depend on, and turns provable breaks into findings. There is no LLM in this job, and no Nebius or other model API calls.

Context: TraceHound is an AI PR reviewer. Its reviewers see the diff and `git grep` references, and they miss contracts broken in unedited files. The analyzer already has TypeScript extractors (`packages/analyzer/src/extract/`: HTTP routes, BullMQ queues, Redis, env, imports, symbols), a declaration-level diff (`changes/`) and graph impact (`impact/`). Read `CLAUDE.md`, `AGENTS.md`, `packages/analyzer/src/changes/` and `packages/analyzer/src/impact/` first.

## Step 0 (if any check fails, STOP and report)
- Branch `cc/contracts-v1` from `origin/main`, which contains `2af8c3a`.
- `pnpm install --frozen-lockfile`, `pnpm test` and `pnpm typecheck` are green before any change.

## Build (one commit per item, each with tests; keep `pnpm test` and `pnpm typecheck` green; stage by explicit path)

1. **`contracts/typed.ts`: the tsc base/head diff (TS repos).**
   - Create two `git worktree`s, at base (the merge base of base and head) and at head. Build a TypeScript program from each repo's `tsconfig.json` files with ts-morph or the compiler API. Don't install the target repo's dependencies.
   - Collect diagnostics and keep those **new at head**: match on file, diagnostic code and normalized message. A line shift is not a new error.
     - Unresolved external modules fail the same way on both sides and cancel out. Test that they do.
   - Split the new diagnostics into `editedFiles` and `uneditedFiles`.
   - Budget: a wall-clock limit (default 120 s) and a file cap (default 5,000). If either is exceeded, return `{ skipped: "budget" }`. Never return a partial result labelled as complete.

2. **`contracts/keyed.ts`: string-keyed contracts (any language, TS-assisted).**
   - Find keys the PR changes: HTTP route paths, queue or job names, Redis key literals or prefixes, env var names, and event or channel names.
     - For TS files, use the existing extractors on base vs head.
     - For other languages, use a conservative literal diff: a quoted string literal removed or renamed in the diff that matches a key-like shape. Write the shapes in `contracts/shapes.ts`, each with tests.
   - For each removed or renamed key, list the files the PR did not edit that still use the **old** key, using a `git grep -F` of the exact literal at head.
   - Output: `{ key, kind, before, after?, staleUses: [{path, line, text}] }`.

3. **`contracts/dependents.ts`: dependents of changed exported declarations.**
   - Using `changes/` declarations (TS) or `git grep -w` of the changed name (other languages), list call or use sites in unedited files, and record the declaration's signature at base and at head.
   - This is data for a later LLM specialist. It creates no findings itself.

4. **`contracts/findings.ts`:** deterministic candidates.
   - `source: "tsc"`: one per new diagnostic in an **unedited** file.
   - `source: "contract"`: one per stale use of a removed or renamed key.
   - Each candidate is `{ path, line, body, source, evidence: [{ path, line, quote }], severity: "high", category: "contract" }`. The quote is the exact line text, and there is one evidence item on each side: the change and the stale use. The body follows the template **What breaks / Where / Evidence**, under 4 sentences.
   - Declare the candidate type locally in `contracts/`; a later integration job unifies it.

5. **CLI:** `tracehound contracts --repo <git dir> --base <sha> --head <sha> --out <file.json> [--budget-s 120]`. It returns `{ typed, keyed, dependents, findings, timings }`. Add one line to `bin.ts`.

6. **Tests.** Build temporary git repos inside the tests. Each break gets a positive case, and each comes with a **control**:
   - a function gains a required parameter / control: it gains an optional one;
   - an exported field is removed / control: it's added;
   - an export is renamed with a stale importer / control: it's renamed everywhere;
   - a return type becomes nullable and a caller dereferences it (tsc strict) / control: the caller already guards it;
   - a queue name is renamed in the producer only / control: it's renamed on both sides;
   - an env var is renamed in config only / control: it's renamed in all readers;
   - an HTTP route path changes on the server only / control: the client fetch is updated too;
   - Python and Go fixtures for the keyed path (string literals only).
   - Controls must produce **zero** findings.

7. **Runtime check** (real public repos; record the timings in the PR body):
   - `SunnyBagal/Linea` and `SunnyBagal/Recall`, on 3 recent commits each: run contracts on `HEAD~1..HEAD` and report the counts and the time.
   - `excalidraw/excalidraw`, as a medium-size TS monorepo, for the same.
   - **Don't use cal.com, calcom/cal.com, sentry, grafana, keycloak or discourse in any way.** Those are benchmark repos.

8. **Docs:** `docs/contracts.md` covers what each part detects, what it can't (behavioural changes that still compile; dynamic keys), the budgets, and that tsc checks duplicate CI wherever a repo already typechecks. Its value is in **unedited files and string keys**.

## Rules
- Touch only `packages/analyzer/src/contracts/`, `packages/analyzer/test/contracts*.test.ts` and `packages/analyzer/test/fixtures/contracts/`, one line in `bin.ts`, and `docs/contracts.md`.
- Don't change `review/`, `gate/`, `packages/action/`, `scripts/bench/`, prompts or `config/`.
- Use `ts-morph` (already an analyzer dependency) or the `typescript` compiler it bundles. Add no other dependency.
- No force-push, no merge. Open the PR to main **as a draft**, titled `contracts-v1: cross-file contract map and deterministic findings`.

End with: the PR number, the head sha, one line per commit, the test count, a table of fixture → expected → got, and the runtime table.
