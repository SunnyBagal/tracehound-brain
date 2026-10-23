# research-01: how do ReviewBench's top reviewers reach high grounded F1, and what could TraceHound change in 2 days?

Tool: **a new claude.ai chat with Research mode on.** Paste everything below the line. When it's done, paste its report into the planning chat.

---

I'm building **TraceHound**, an open-source AI pull-request reviewer: a GitHub Action on NVIDIA Nemotron via Nebius Token Factory. I'm entering it on **ReviewBench** (review-bench.ai; repo github.com/review-bench/ReviewBench; GitHub's blog post "ReviewBench: an open benchmark for AI code review"). I need a research report that tells me what architecture changes would raise our **grounded F1** substantially, and fast: I have about 2 working days.

## How ReviewBench scores (from its methodology doc; please confirm against the repo)
- The golden set has about 4,600 findings over 219 PRs, about 12 true positives per PR. It was built from human reviewer comments, **inferred author actions** (what the author fixed in follow-up commits), **deterministic tools (e.g. Semgrep)** and several LLM reviewers. A Claude Sonnet classifier labelled every finding TP or FP, and humans then corrected the labels.
- An LLM matcher compares a candidate finding with golden findings **in the same file only**, deciding whether they describe "the same underlying issue".
- **Grounded precision** = matched-to-a-golden-TP / all matched. **Unmatched candidate findings are excluded from both numerator and denominator.** **Grounded recall** = golden TPs covered / all golden TPs. So grounded F1 is driven by recall; the leaderboard's precision is 84–90% for everyone.
- Augmented metrics are reported separately: a classifier labels unmatched findings and penalises FPs.

## Where we are (one 25-PR test run, 23 Oct)
- Config: two Nemotron 3 Super generators (diff-only, and repo-context), pooled, **no verifier**.
- About 5 findings per PR.
- **Grounded F1 16.0, P 70.6, R 9.0.** Augmented F1 23.0 (P 52.5, R 14.7).
- Leaderboard (grounded F1, full set):
  - Laude RevEngine Prime (Opus 5.5) 59.6
  - block/review 47.7
  - Klaussy (Sonnet 5.5) 44.1
  - Copilot 40.1
  - DeepSeek Harness 39.9
  - Devin 37.0
  - Qodo 35.1
  - Codex 30.4
  - Cubic 27.3
  - Greptile 27.2 (P 86.1 / R 16.2)
  - Cursor 17.3 (P 87.7 / R 9.6)

  Please verify these numbers and add any newer entries.
- **Constraints:**
  - the models must be NVIDIA Nemotron on Nebius Token Factory (3 Super, 3.5 Super VL; maybe 3 Ultra 550B);
  - Token Factory budget about $20–40 for a final run of 219 PRs × 3 rounds;
  - 1800 s per PR;
  - no network except Token Factory;
  - a Docker image;
  - **we never look at ReviewBench's golden set or tune on it**, by our own pre-registration, even though ReviewBench's rules allow it.

## What I need
1. **What the top entries do.** For each of the top 6 entries plus Copilot, Greptile and Cursor, what is publicly known about their architecture: one pass or many, per-file or whole-diff, how many findings per PR, specialised "lenses" or agents (security, correctness, tests…), static tools in the loop, tool use or agentic repo exploration, self-consistency or sampling, verifiers or filters, and the model. Check whether block/review is open source, and if so, read its code and prompts. Cite every claim, and mark UNVERIFIED anything you couldn't open.
2. **ReviewBench specifics that matter for recall.** From the repo, without opening the golden data:
   - the agent contract (finding format, line ranges, maximum findings, severity fields);
   - the matcher prompt (what makes two findings "the same issue"; whether message detail or line precision matters);
   - whether a cap on findings or a duplicate penalty exists;
   - how "inferred author actions" and tool findings were produced (which Semgrep rule sets?);
   - the classifier's FP guidelines (which kinds of finding are labelled FP, so we avoid matching them).
3. **Evidence-backed levers to raise recall without wrecking precision,** ranked by expected gain per day of work and per dollar. For each: what it is, evidence (papers such as CR-Bench arXiv 2603.11078, vendor blogs, ablations), expected effect size, cost multiplier, and risk. Consider at least:
   - per-file fan-out;
   - multiple diverse review lenses with a union;
   - sampling at temperature 1.0 / top_p 0.95 (NVIDIA's recommendation for Nemotron) with a union across samples;
   - a missed-issue pass;
   - static analysers (Semgrep with bundled offline rules, ESLint, ruff, tsc, gitleaks) as a deterministic finding source;
   - "what would the author fix next" prompts (aimed at inferred author actions);
   - test-coverage and docs findings;
   - a reasoning budget / `max_tokens` (we lose about 1 in 6 chunks to truncation on 3.5);
   - message format for the matcher;
   - dedup;
   - Nemotron 3 Ultra vs Super.
4. **A concrete 2-day plan** for a "recall mode" ReviewBench config, v3: the architecture, the prompts' outline, a cost per PR estimate, and how to validate it **without** ReviewBench's golden set (we have our own 40-PR dev set with human-comment golden labels from `ai-code-review-evaluations/golden_comments`), plus what to disclose.
5. **Honesty check:** where does optimising grounded F1 diverge from being a useful reviewer, and how do the top entries' augmented precision compare? We publish both metrics.

Output: a report with sections 1–5, a ranked lever table (lever, expected Δ grounded recall, cost ×, days, evidence link), and a sources list. Be direct; say plainly when the evidence is thin.
