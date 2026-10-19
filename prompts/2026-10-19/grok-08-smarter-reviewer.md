# grok-08: what would make an open-model PR reviewer smarter in 10 days

Tool: **Grok Premium, a fresh chat.** Optionally paste the same text into **ChatGPT (deep research)** as a second, independent pass; Plus runs until 22 Oct. Paste everything below the line. Send both answers back to the planning chat.

---

I'm building an open-source AI pull-request reviewer. It runs on **NVIDIA Nemotron 3 Super (120B MoE, 12B active) via Nebius Token Factory**, with a 10-day deadline (29 Oct 2026) and about $40 of inference budget. I can't train on GPUs, and full RL fine-tuning is out of scope. I need **evidence-backed** techniques, not opinions.

**Format rules:** use live web search. For every claim, write the full URL as **plain text inside the table cell** (no citation chips or markdown links), plus the page's date. Mark anything you couldn't open as UNVERIFIED. If you can't find something, write "not found".

## Current pipeline (for context)
Two generator passes run over the diff: one diff-only, and one with repository context from `git grep` of the changed names. Their findings are pooled, and one verifier pass keeps or drops each finding. All calls use Nemotron 3 Super at temperature 0.

What we measured:
- recall is decent (44–51% on a 40-PR set);
- precision is ~40%;
- run-to-run F1 spread is 12 points;
- voting across 5 runs and an "evidence-quote" verifier both failed to raise F1.

The next benchmark (ReviewBench, 219 PRs) scores mostly on **recall**, against about 12 golden issues per PR. Those issues span correctness, reliability, security, API design, tests and maintainability.

## Q1. Techniques that raise recall or quality without training (2024–2026 evidence)
For each technique, give: a one-line description, a source URL and date, the measured gain (with the numbers), and the cost multiplier.
Cover at least:
- specialized passes per category (security / concurrency / API contract / tests);
- per-file or per-hunk review vs whole-diff;
- self-critique or reflection loops;
- few-shot exemplars;
- "think about the minimal fix" prompting;
- multi-sample union vs voting;
- reasoning-budget settings;
- retrieval of callers and callees;
- running linters or type checkers as tools.

Then rank the top 5 for **recall per dollar**.

## Q2. Learning from mistakes without weight updates
How do production reviewers (Greptile, CodeRabbit "learnings", Cursor Bugbot rules, Qodo, Graphite) turn feedback into better future reviews? Cover:
- memory of rejected comments;
- LLM-distilled per-repo rules;
- retrieval of past accepted comments as few-shot examples.

Give the published numbers. Then answer: which of these can an open-source GitHub Action implement with a git branch as storage and no server?

## Q3. Light training options, as a feasibility check only
- Does **Nebius Token Factory** offer fine-tuning or LoRA for Nemotron 3 Super or Nemotron 3.5 models? Give the URL, price and turnaround.
- Is **Nemotron 3.5 Super** available on Token Factory? Give the model id, price, and any published code-review or SWE-bench numbers against Nemotron 3 Super.
- What are NVIDIA's recommended sampling settings for Nemotron 3 / 3.5 Super, and the reasoning-budget controls?
- Are there published results of small-scale RL or preference tuning (DPO/KTO) **for code review comments**? Give the paper, year, data size and gain.

## Q4. Your recommendation
Name the 3 changes most likely to raise ReviewBench grounded recall by **≥ 5 points** within 10 days and $40. Then name 2 things that sound good but won't move it, and say why.

Keep it under 1,500 words, excluding tables.
