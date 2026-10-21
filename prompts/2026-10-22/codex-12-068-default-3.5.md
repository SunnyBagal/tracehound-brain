# codex-12 (decision 068): make 3.5 the Action's default, and stop throwing away a whole review over one failed chunk

Tool: **ChatGPT Codex**, a **new task**, in `~/Projects/TraceHound-codex`. Start only after **#56 is merged**. Paste everything below the line.

---

Decision 067 (#56) measured `pool-verify-v1` on `nvidia/Nemotron-3.5-Super-VL` on dev and **adopted it** under its pre-registered rule: v1 F 50.4 vs 42.3, P 54.8 vs 40.5, x1 F 55.1 vs 51.3, on 2 runs.

3.5 returned invalid JSON twice on a reviewer chunk on 7 of 40 PRs per run. The bench runner scored those PRs **with partial results**: a `CHUNK_FAILED` chunk contributes nothing, and everything else in the PR is kept (`packages/analyzer/src/review/review.ts:5`). Only 3 of 40 PRs ended with 0 findings.

**The Action doesn't do that.** `packages/action/src/pipeline.ts` throws on `chunkFailed > 0` in either reviewer, and `action.ts` posts nothing. With 3.5, about 1 in 6 PRs would get **no review at all**, and that is not the configuration that was measured. The ReviewBench adapter (`scripts/reviewbench/entry.ts:123–130`, all-or-nothing passes) has the same mismatch.

This job makes the product match what was measured, then switches the default.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` is empty. Branch `codex/068-default-3.5` from `origin/main`, which must contain #56's merge.
- `pnpm test` and `pnpm typecheck` are green, run in a plain shell; the `GIT_*` env check prints nothing.
- Read:
  - `packages/action/src/pipeline.ts`, `action.ts` and `cli.ts`;
  - `.github/actions/tracehound-review/action.yml`;
  - `packages/analyzer/src/review/review.ts`;
  - `scripts/reviewbench/entry.ts` and `adapter.ts`;
  - `docs/action/README.md`;
  - `docs/bench/reviewbench.md`;
  - `docs/bench/dev-3.5.md`.
- **Don't open** `linea-seeded`, `recall-seeded`, `tracehound-seeds`, or anything under ReviewBench's `golden/`.

## What to build (one commit per item, each with tests, no network)
1. **Partial results in `runPipeline` (pool-verify-v1).**
   - A reviewer whose output has `chunkFailed > 0` no longer throws. Its successful chunks' comments are used as they are.
   - The pipeline fails (returns `null`, nothing posted) **only** in these cases:
     - **both** reviewers failed every chunk;
     - a process error;
     - invalid output.
   - The verifier is unchanged: VERIFY_FAILED findings are still kept.
   - The footer record gains `chunkFailed: { diff: n, ctx: n }` when either is non-zero, and the review body gains one line: `N of M reviewer chunks failed (invalid model reply); this review is partial.`
   - `review-diff-v1` mode is unchanged.
   - Tests:
     - one reviewer has a failed chunk and the other is OK: posts, with the line and the footer field;
     - both reviewers fail every chunk: nothing is posted;
     - nothing failed: the output is **byte-identical** to today's.
2. **The same rule in the ReviewBench adapter.**
   - `entry.ts`: `pool-raw`, `pool-raw-x2` and `pool-raw-missed` keep the successful chunks' findings instead of writing `[]`.
   - A pass fails only when all of its reviewer chunks failed.
   - The status line says `status=ok partial=<failed>/<chunks>` when partial. The smoke workflow must still go red on `status=failed`, and must treat `status=ok partial=…` as ok: update the check there, and its comment.
   - `pool-verify-v1` in the image already uses `runPipeline`, so item 1 covers it.
   - Adapter tests: `pnpm exec vitest run --config scripts/reviewbench/vitest.config.ts` and `pnpm exec tsc -p scripts/reviewbench`.
3. **Default model.** In `.github/actions/tracehound-review/action.yml`, the `model` input default becomes `nvidia/Nemotron-3.5-Super-VL`. That model goes to every reviewer and verifier stage through `--model`. Leave unchanged:
   - `FIX_MODEL` (fixes stay on 3 Super; that stage wasn't measured on 3.5);
   - the analyzer CLIs' own defaults (`DEFAULT_MODEL`);
   - the bench `REVIEW_MODEL`;
   - prompts;
   - `max_tokens`.

   An explicit `model: ''` keeps today's per-stage defaults. The client routes 3.5 to us-north1 (#55), so nothing else is needed. Test the manifest default.
4. **Docs.**
   - `docs/action/README.md`: the default model, the partial-review rule, the footer field, the cost (median about $0.06 per PR vs $0.03, from `docs/bench/dev-3.5.md`), and that fixes stay on 3 Super.
   - `docs/bench/reviewbench.md`: the adapter's partial rule.
   - Append **decision 068** to `docs/decisions.md`:
     - choice: 3.5 as the default, with partial results on failed chunks, to match how 067 was measured;
     - rejected alternative: keep all-or-nothing, which would post nothing on about 1 in 6 PRs with 3.5;
     - rejected alternative: a 3 Super fallback on failure, which is unmeasured.

## Rules
- Touch only:
  - `packages/action/src/` (`pipeline.ts`, `action.ts`, `cli.ts`) and `packages/action/test/`;
  - `.github/actions/tracehound-review/action.yml`;
  - `scripts/reviewbench/` and its tests;
  - `.github/workflows/reviewbench-smoke.yml` (the status check only);
  - `docs/action/README.md`, `docs/bench/reviewbench.md`, `docs/decisions.md` (append only).
- **Nothing in `packages/analyzer/src`**, `config/`, `scripts/bench/`, or any prompt.
- Keep `cleanGitEnv` and `contractsEnv`. No new dependencies.
- Make no Nebius calls, and no live check: CI and the dogfood review on the PR are enough.
- Open the PR as a **draft**, titled `068: Nemotron 3.5 default + partial reviews on failed chunks`. Wait for CI; make one fix attempt if it's red. No force-push, no merge.

End with:
- the PR number, head sha, and one line per commit;
- test counts (workspace and adapter);
- the byte-identical result when nothing fails;
- anything you couldn't do.
