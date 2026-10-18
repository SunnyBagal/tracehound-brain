# codex-08b: two gate fixes before #45 merges

Tool: **ChatGPT Codex**, the **same task** that built #45 (`~/Projects/TraceHound-codex`, branch `codex/gate-v1`). Reply in that task with everything below the line.

---

Good work, the planning chat checked #45. Two problems must be fixed before merge. Both come from my brief, not your code. Same rules as before: one commit per fix, each with a test that fails before the fix, `pnpm test` and `pnpm typecheck` green, stage by explicit path, no force-push, and the PR stays a draft.

## 1. The `style` rule drops real bugs
I ran `nitMatch` on bug reports:
- "This dereferences `user` which can be null after the change; consider guarding it or the request crashes with a TypeError." → dropped as `consider`.
- "Typo in the env var name: `DATABSE_URL` is never set, so the pool connects to localhost in production." → dropped as `typos`.

Fix:
- Add `defectSignals` to `gate/nits.ts`, a reviewed list of words that indicate a runtime defect, written from general knowledge and with no benchmark data. It covers crash, throw/throws, exception, TypeError, null, undefined, NaN, panic, fail/fails, error, wrong, incorrect, broken/breaks, never, always, leak, race, overflow, off-by-one, injection, XSS, unauthorized, bypass, data loss, deadlock, infinite loop, regression, "is never set", "returns … instead of".
- `style` drops a finding only when `category === "style"`, or when a nit pattern matches **and no defect signal does**.
- `category === "contract"` or `source` `"contract"`/`"tsc"` is never dropped by `style`.
- Add the two sentences above as fixtures that must be **kept**. Also add "Consider renaming this variable for clarity." and "nit: trailing whitespace" as fixtures that must be **dropped**.

## 2. Evidence sides and null lines
- **Base-side evidence.** The contracts stage (#47, `packages/analyzer/src/contracts/findings.ts`) emits evidence items with `side: "base" | "head"`. A removed key's line lives only at base. Your gate checks every quote against the head, so it would drop every contract finding as `evidence-mismatch`.
  - Add an optional `side` to the evidence type and an optional `readBaseFileAt` to `GateContext`.
  - Check `side: "base"` quotes against the base. If no base reader is given, skip base-side quotes (don't drop them) and say so in `detail`.
  - The CLI gets an optional `--base <sha>` and reads base files with `git show <sha>:<path>`, using the env from `cleanGitEnv()`.
  - Add a test using a candidate shaped exactly like `ContractCandidate` from #47: copy the shape, don't import from `contracts/`.
- **Null lines.** `not-in-diff` currently drops every finding with `line: null`. A null line is a file-level comment, which the reviewer schema allows. Keep it when its path is in the retained diff, and drop it as `not-in-diff` otherwise. Add a test for each case.

Update `docs/gate.md` for both fixes. End with: the head sha, one line per commit, the test count, and the fixture table for the four sentences above.
