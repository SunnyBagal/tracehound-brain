# devin-01: whole-codebase review + ideas to raise the benchmark numbers (report only)

Tool: **Devin**, a **new session** on `SunnyBagal/TraceHound` (main `5ba60a0` or later). Paste everything below the line. Sunny pastes Devin's final report to the chat.

---

TraceHound is an open-source AI PR reviewer (GitHub Action on NVIDIA Nemotron via Nebius Token Factory) plus a component graph of TypeScript repos, built for a hackathon that closes 30 Oct. **This is a read-and-think job: no PRs, no commits, no branches, no model or API calls, no paid anything.** Your output is one report.

## Two questions
1. **Code:** where is the codebase weakest? Real bugs first, then risky code, then things a reviewer reading the repo would trip on. Cover `packages/analyzer` (esp. `src/review/`, `src/llm/`, `src/contracts/`), `packages/action`, `.github/`, `scripts/bench/`, `viewer/`.
2. **Numbers:** what would most likely raise TraceHound's **F (precision and recall)** on its benchmark, and how would we test it cheaply? Search the web for how other AI code reviewers and recent papers cut false positives and raise recall (verification, ranking, self-consistency, structured output, context retrieval, severity filtering, etc.) and map each idea onto **our** pipeline.

## Read first
- `CLAUDE.md`, `README.md`, `docs/bench/results.md`, `docs/bench/dev-3.5.md`, `docs/bench/levers-v2.md`, `docs/bench/v3-recall.md`, `docs/bench/dev-ultra.md`, and the decisions 055–071 in `docs/decisions.md`.
- The pipeline (`pool-verify-v1`): `review-diff-v1` + `review-ctx-v1` → pool → `verify-ctx-v1` → fixes. Product model `nvidia/Nemotron-3.5-Super-VL`, frozen at tag `review-freeze-2` (`0aeef06`).

## Already known (don't re-report; build on them if you have something new)
- **Tried and lost or not adopted:** confidence-threshold verifier (056), context reviewer alone (057), context verifier alone (058), vote across 5 runs (064), evidence-quoting verifier + gate (065), recall-first lenses + missed-issue pass (070), Nemotron 3 Ultra (071), per-PR cap / verifier on-off / 2-of-3 vote replays (levers v2). An idea that repeats one of these needs a concrete reason it would come out differently.
- **Open problems:** 3.5 returns invalid JSON on ~7 of 40 dev PRs per run and 79 of 624 calls hit the 32,000-token limit (reasoning likely eats the budget); run-to-run spread at temperature 0 (35.6–48.0 F) is bigger than most effects; all runs used temperature 0, though NVIDIA recommends 1.0; ReviewBench's metric rewards volume while our verifier cuts it; both 3.5 runs miss 35 of 74 Medium+ golden comments (sentry worst).
- **Backlog:** JSON repair / schema-constrained output; truncation handling in the Action; no-post log line with pool/kept counts; merge-base fetch duplicated in `viewer/lib/data.ts`; contracts compiler-read containment; 2 known recall-v3 defects (documented, v3 is dead).

## Rules
- **The Cal.com test set is blind.** Don't open, quote or reason from Cal.com golden comments or Cal.com review text anywhere (the benchmark dataset, docs, PR comments). Same for ReviewBench `golden/`, and the repos `linea-seeded`, `recall-seeded`, `tracehound-seeds`. Ideas must be testable on the **40-PR dev set only**.
- **No tuning to the answer key.** Don't propose prompt edits that target specific golden comments or repos. Ideas must be general.
- The product **does not change before 30 Oct**. Everything you propose is for after the hackathon (or a $0 offline replay on saved dev records, which we run ourselves).
- Every claim about the code needs a file and line. Mark anything you didn't verify as "unverified".

## Report (final message, under 150 lines)
1. **Bugs** (severity, file:line, what breaks, how to reproduce, minimal fix). Max 10, worst first.
2. **Code improvements** (file:line, why, effort S/M/L). Max 10.
3. **Ideas to raise F**, ranked by expected gain ÷ cost. For each: what it is, the source (link) it comes from, where it plugs into our pipeline (file), whether it targets precision or recall, **the cheapest honest test** ($0 replay on saved dev records / a dev run with an estimated $ cost / needs new data), the result that would count as a win against the run-to-run spread, and the main way it could fool us (e.g. volume gaming, judge bias, overfitting dev). Max 8.
4. **One recommendation**: the single next experiment you'd run first, and why.

Sources: list every web link you used.
