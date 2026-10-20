# 066c report (Bunny, 2026-10-20)

**PR #52** (draft, not merged): https://github.com/SunnyBagal/TraceHound/pull/52
**New head:** `e91a268dc9bbef37938afa448cad846df482ddb0` (pushed; was `0d3a6c4`)

## Commit
- `e91a268` 066: close the three open items from the #52 report

## Changes (only these strings)
- `viewer/app/benchmark/page.tsx`:
  - "The judge and reviewer share a model family." → "The judge is the same model (Nemotron 3 Super) as the reviewer and the verifier."
  - "decision 063" → "decisions 063, 066".
  - Nothing else in the file changed (checked with a word diff).
- `CLAUDE.md:250` (062 entry): "pool-verify-v1 ranks 3rd of six:" → "pool-verify-v1 was 3rd of six on run 1 (a point estimate; see decision 066):".
- `docs/bench/results.md:158` (§6 temperature sentence): the model-card link is now https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-BF16 (was the 3.5 Super VL card). The NIM reference link is kept.

## Tests (run as their own command, before the commit)
- pnpm test: action 179/179; analyzer 849 passed, 7 skipped (856); viewer 188/188.
- pnpm typecheck: clean.
- Load average was 6, and there were no timeouts.

## Note
- `BENCHMARK_REVISION` in `viewer/lib/benchmark.ts` still pins `e54ee63`. That revision's results.md has identical tables, but the old 3.5 VL link in §6 prose. Re-pinning to `e91a268` would take another commit, and this job was one commit, so I left it.
- CI on `e91a268` had not been checked when this report was written.
