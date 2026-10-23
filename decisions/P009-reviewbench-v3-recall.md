# P009 · ReviewBench v3 "recall mode": pre-registration

Written by chat, 23 Oct 2026 22:45 IST, **before any v3 code or model call exists**. Builder: relay job [070](../prompts/2026-10-23/070-v3-recall-mode.md). Evidence: [R16](../research/R16-reviewbench-grounded-f1.md).

## Disclosure, written first
- v3 was designed **after** seeing one P008 test-run aggregate (candidate 2: grounded F1 16.0, P 70.6, R 9.0). No per-PR detail and no golden data was used.
- R16 cites third-party statistics that someone else computed from ReviewBench's `golden/` (producer shares, category gaps). **v3's design does not depend on them.** Its levers come from ReviewBench's methodology, its README's test-set category table, and public leaderboard A/B rows. Results state that R16 mentioned them.
- v3 is a **benchmark configuration**. The Action's default stays pool-verify-v1 at `review-freeze-2`. Results say so.

## What v3 is (the builder fills the details; the shape is fixed here)
A new pipeline `recall-v3` in the ReviewBench adapter and the analyzer. It's additive: existing pipelines stay byte-identical.
1. **Prep, deterministic:**
   - parse the diff;
   - drop lockfiles, generated, minified and binary files;
   - pack per file into chunks of ≤ ~8k diff tokens, split at hunk boundaries, with head line numbers;
   - at most 8 chunks per PR, prioritised source > tests > config > docs; skipped files are logged.
2. **Lens A, one call per chunk:** correctness, reliability, security, performance, API/compatibility.
3. **Lens B, one call per chunk:** tests (missing or ineffective, naming the regression), stale docs/comments, user-facing typos, maintainability, completeness.
4. **Missed-issue pass, one per PR:** the existing frozen `review-missed-v1` (sha `a5a2115f…`) over the deduplicated A+B findings. Findings are tagged by producer, so "without the missed pass" can be scored from the same run.
5. **Prompts:**
   - every real, verifiable issue, low severity included;
   - no quota, never invent;
   - one issue per finding; quote the line relied on;
   - anchor to the file where the fix belongs;
   - message form "`<symbol>`: <what is wrong> → <consequence>. Fix: <minimal change>.";
   - PR title and body fenced as untrusted.
   - The lens prompts' sha256 values are recorded in the repo plan **before** the first dev call.
6. **Sampling and reasoning:**
   - temperature 1.0, top_p 0.95 (NVIDIA's recommended setting);
   - a reasoning budget if Token Factory honours one (the builder checks with one call), else max_tokens 16k with one retry on `finish_reason=length` with reasoning off.
7. **Reduce, deterministic:**
   - drop paths missing at head;
   - clamp ranges to the file;
   - a null line is anchored to the file's first changed line;
   - merge duplicates (same file, overlapping lines, similar text);
   - cap at 60;
   - never fail the PR: on error, write the findings so far, or `[]`. ReviewBench fails the whole run if one PR never succeeds.
8. **Hard per-PR budget:** $0.12, and a wall-clock guard well under 1800 s.

## Dev selection (40 dev PRs; no Cal.com; no ReviewBench data)
- **Arms:** `recall-v3` on `nvidia/nemotron-3-super-120b-a12b` (**S**) and on `nvidia/Nemotron-3.5-Super-VL` (**V**). One dev run each, scored by bench-judge-v1 (scorer of record) with x1 alongside.
- **Baselines:** pool-raw's dev rows on the same model, from the existing dev runs (3 Super: runs 1, 2r, 4, 5, 6; 3.5: 7r, 8). The builder recomputes them and states them.
- **Floor:** an arm qualifies only if its dev v1 **precision ≥ (its model's pool-raw dev mean precision − 10.0 points)**.
- **Pick:** the qualifying arm with the higher dev v1 **recall**. A tie within 2.0 points goes to the cheaper per PR. **If neither qualifies, v3 stops here** and the P008 winner gets the final.
- **Reported, not deciding:** the same numbers without the missed-pass findings, and without lens B; cost and seconds per PR; truncation counts.

## ReviewBench
- A new image is built from the v3 merge. The pick gets **one smoke run** (indexes 0 and 4, green), then **one portal config** (`pipeline=recall-v3`, `model=<pick>`) and **one 25-PR test run**.
- **Final rule (with P008 amendment 7):** v3 gets the one final run only if its test-run **grounded F1 ≥ the P008 winner's test-run grounded F1 + 3.0**. Otherwise the P008 winner does.
- **Cutoffs:** if v3 can't reach its portal test run by **25 Oct 23:59 IST**, it's dropped and recorded as not run. It never delays the final past 26 Oct.
- **Budget:** the relay dev cap is $14. The local total cap rises from $45 to $60 (Sunny's console credit; Sunny to confirm). A v3 final is estimated at about $33 (R16, unmeasured): the builder replaces this with the dev-measured cost per PR × 657.

## Disclosure list (results page)
Config name, model and endpoint, sampling, reasoning budget, chunk cap, lenses, missed pass, no verifier, finding cap, measured cost and time per PR, the dev-set size and its labels' origin, that golden data was never opened, R16's golden-derived citations, the number of configurations tested, and that v3 came after seeing one test-run aggregate.
