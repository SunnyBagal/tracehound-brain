# codex-13b: resume the hostile-PR audit (Step 0 timeouts are load flakes)

Tool: **ChatGPT Codex**, the **same task** as codex-13 if it's still open, else a new one in `~/Projects/TraceHound-codex`. Paste everything below the line, then the original codex-13 brief below it if it's a new task.

---

Your Step 0 stop was correct by the letter of the brief. But both failures were **5,000 ms timeouts under load**, not assertion failures:
- `contracts-fixtures.test.ts` (positive: required parameter added);
- `test-set-run.test.ts` (061a restart refusal).

Bunny hit the same kind of timeout in `contracts-findings.test.ts` today, and its immediate rerun was green. The planning chat ran `test-set-run.test.ts` alone on Node 22 at a newer commit: 50/50 green.

**Amended Step 0 gate (replaces "`pnpm test` green"):**
1. `pnpm typecheck` is green.
2. `pnpm test`. If every failure is a `Test timed out in 5000ms`, rerun only the failing files with a longer timeout:
   ```
   cd packages/analyzer && pnpm exec vitest run <file> --testTimeout 30000
   ```
3. If those pass, record the flake (file, test name, both results) and **continue with the audit**.
4. Stop only on an assertion failure, a typecheck error, or a timeout that still fails at 30 s.

Don't re-run the whole suite more than once; it costs you half an hour. Everything else in codex-13 is unchanged: read-only, `/tmp` fixtures, no network, report to `inbox/2026-10-23-codex-13.md` (overwrite your stopped report; it's already filed as `prompts/2026-10-23/codex-13-hostile-pr-audit.out-1-stopped.md`).

**One extra question (Q9), because a public claim depends on it:** the Devpost draft (PR #58, `docs/devpost/writeup.md`) says the contracts stage runs "no script from the pull request". Is that true, given tsconfig `plugins`, `extends` into PR-controlled `node_modules`, and anything TypeScript's API might load? Answer **true / false / true with caveats**, with evidence.
