# cc-02: can we run TraceHound on GitHub ReviewBench? (Claude cloud session)

Tool: **Claude Code on the web (claude.ai/code), a new cloud session on repo `SunnyBagal/TraceHound`, branch `main`.** Paste everything below the line. **Make no model calls: no Nebius key here, and no Anthropic API calls.**

---

Our current benchmark has only 10 blind PRs, which critics will rightly call too small. GitHub ReviewBench (announced 5 Oct 2026: https://github.blog/ai-and-ml/github-copilot/reviewbench-an-open-benchmark-for-ai-code-review/) is open, with 219 PRs, 187 repos, 19 languages, and a public dataset, rubric, judge and runner. Find out whether we can score TraceHound on it, and what that would cost.

Do:
1. Find the dataset and runner from the blog post. Report:
   - their URLs and licence;
   - the data format;
   - how a third-party reviewer plugs in (what it must output);
   - the judge model and how it is called (which API, how many calls per PR);
   - whether published results for other tools exist, and which tools;
   - the language mix. TraceHound's repo-context reviewer may favour TypeScript.
2. Read TraceHound's reviewer entry points (`packages/action/src/pipeline.ts` and `scripts/bench/run-test-set.ts`) and design an adapter: ReviewBench PR → our pipeline input → ReviewBench submission format.
3. Write a **no-model-call** adapter skeleton under `scripts/reviewbench/` with unit tests on fixture data (a hand-made 2-PR fixture; do not commit their dataset unless its licence allows it).
4. Estimate the cost of one full run:
   - our side: about $0.03–0.05 per PR on Nebius for pool-verify-v1, or 5× that with voting;
   - their judge side: tokens per PR × the judge's price.
   Give totals for all 219 PRs and for a stratified 60-PR subset.
5. Write `docs/bench/reviewbench-spike.md` with all findings, then open a **draft** PR titled `bench: ReviewBench feasibility spike (cc-02)`.

Rules:
- Only touch `scripts/reviewbench/` and `docs/bench/reviewbench-spike.md`.
- Small commits, each with `pnpm test` and `pnpm typecheck` green. Stage by explicit path.
- Never merge. End with a summary: feasible yes/no, blockers, the cost table, and a recommended subset.
