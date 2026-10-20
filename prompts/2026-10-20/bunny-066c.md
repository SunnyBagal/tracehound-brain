# Bunny job 1c: three small fixes on #52, then done (paste into the SAME Bunny session)

Where: **Bunny**, the same session, `~/Projects/TraceHound-claude2`, branch `docs/066-results` (#52, head `0d3a6c4`). Sonnet. Paste everything below the line.

---

The chat checked #52. Scope is fine (8 files, all allowed), CI is green, and the numbers recompute. Add **one commit** with the three open items from your report, then stop:

1. `viewer/app/benchmark/page.tsx`: the line "The judge and reviewer share a model family" → "The judge is the same model (Nemotron 3 Super) as the reviewer and the verifier". "decision 063" → "decisions 063, 066". Only those strings; nothing else in that file. (This is allowed now.)
2. `CLAUDE.md:250` (the 062 entry): "ranks 3rd of six" → "was 3rd of six on run 1 (a point estimate; see decision 066)".
3. The temperature sentence in `results.md` §6: replace the 3.5 VL model-card link with the **Nemotron 3 Super** card, https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-BF16, the model our runs used. Keep the NIM reference link.

Run `pnpm test` and `pnpm typecheck` as their own command, then commit separately, then push. Report the new head sha, and write the report to `~/Projects/tracehound-brain/inbox/2026-10-20-bunny-066c.md` (rules in `inbox/README.md`).
