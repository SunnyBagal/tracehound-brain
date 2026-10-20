# Bunny job 1b: finish 066 (paste into the SAME Bunny session that stopped)

Where: **Bunny** (`claudeb`), the same session, `~/Projects/TraceHound-claude2`, branch `docs/066-results` (head `27990eb` plus your uncommitted step-3 changes). Sonnet is fine. Paste everything below the line.

---

Good stop: you were right to stop instead of editing `viewer/`. The chat's decisions:

1. **Option (b): this PR may also touch exactly two viewer files**, `viewer/lib/benchmark.ts` and `viewer/test/benchmark.test.tsx`:
   - Sync `BENCHMARK_TABLES` with every table in the new `docs/bench/results.md`, including the all-50 tables and the levers table. The test that compares them stays. The viewer also still says "3rd" somewhere (NOW noted it on 19 Oct); fix that to match the new wording.
   - Replace the blanket ban at `:18`. The page may say "all 50 PRs" **only** inside the all-50 table's own heading or caption. Add a check that the page never says "3rd of 6" (or "3rd of six") as a ranking.
   - Nothing else under `viewer/`.
2. **Fix 5's wording is wrong; correct it.** The audit's §3.2 shows both 95% intervals for the CodeRabbit gap include 0, so "Apart from CodeRabbit, every gap is within noise" overstates it. Write instead:
   > "No gap to another tool is outside its 95% interval. The CodeRabbit gap is the clearest: TraceHound is ahead in 96% of resamples, but its interval still includes 0 (audit-cc01 §3.2)."

   Apply the same correction wherever the opening paragraph or the README headline says "ahead of CodeRabbit". The run-1 numbers can still be stated ("58.3 vs 36.7 on run 1"), but not as a significant lead.
3. **Temperature sentence:** keep it, and add the source in place: NVIDIA's model card recommends temperature 1.0 and top_p 0.95 for text (https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16 and the NIM reference https://docs.api.nvidia.com/nim/reference/nvidia-nemotron-3-super-120b-a12b). We run at 0.
4. **Process fix:** run the gates (`pnpm test`, `pnpm typecheck`) as their **own command**, read the result, and only then run `git commit` as a separate command. Never chain a commit after a test with `;`.

Then do steps 3–7 of the 066 brief as written (step 3 with the changes above), push `docs/066-results`, and open the PR as a **draft**.

End by writing your final report verbatim to `~/Projects/tracehound-brain/inbox/2026-10-20-bunny-066.md` (rules in `inbox/README.md`: only that file, then `git pull --rebase`, commit, push). Also print the report here.
