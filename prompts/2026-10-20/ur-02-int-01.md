# ur-02: Ultrareview #2 on PR #53 (int-01, contracts in the Action)

Tool: **Claude cloud** (claude.ai/code), a **new session** on repo `SunnyBagal/TraceHound`, branch `codex/int-01-contracts`. Uses one of the last 2 Ultrareviews. Type the command, then paste the focus text below the line if the session asks for focus (or as the first message after the command starts).

```
/ultrareview 53
```

---

Review PR #53 (head `747a3a7`): it wires the deterministic cross-file contract checker into the TraceHound GitHub Action, default **on**, so it runs on every PR of every repo that installs the Action. Find real bugs; skip style.

Focus, in this order:
1. **Untrusted PR code.** `packages/action/src/contracts.ts` runs `packages/analyzer/src/bin.ts contracts` (TypeScript compiler, git worktrees) over the PR's checkout. Can a PR's files (tsconfig `extends`/`references`/`plugins`, `.gitattributes`, symlinks, huge or deep files, odd paths) make it execute code, read outside the repo, leak env, or hang past the 90 s budget? Is the child env really credential-free (`contractsEnv`, the `-c credential.helper= … http.<server>/.extraheader=` fetch)?
2. **Git correctness.** Merge base vs base tip; the depth-1 checkout → `fetch --unshallow origin <base>` path; fork PRs; force-pushed or stale heads; base sha from the event vs the API.
3. **Fail-soft.** Any path where a contracts failure, timeout or rejection blocks, delays past the job timeout, or changes the model review. `settleContracts` races 90 s; does the child die on timeout? Any unhandled rejection after the race resolves?
4. **Delivery.** `placeContracts` / `contractsSection` / the ±3 dedupe in `action.ts`: caps, "and N more", body-only 422 fallback, Markdown injection from contract reason text (key names come from PR code), link encoding, the footer's `contracts` record and `parseReviewFooter` in the data job.
5. **`contracts: 'off'` byte-identical** to main for both reviewers, fixes on/off, learn on/off.

Already known (don't re-report unless you find it worse than stated): the dedupe also matches contract findings hidden in "and N more"; the reason text isn't Markdown-escaped; a budget/error skip of the typed check drops the keyed findings too; private repos at depth 1 fail soft; the footer omits `status: "off"` when off.

**Don't fix anything in this session.** End with a numbered list: severity, file:line, what breaks, a concrete repro input. Make no Nebius calls.
