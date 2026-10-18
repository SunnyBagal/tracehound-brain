# grok-07: which reviewers we can run for free, and how they learn

Tool: **Grok Premium, a fresh chat.** Paste everything below the line. When it answers, paste the output back to the planning chat.

---

I'm building an open-source AI pull-request reviewer for a hackathon. The deadline is 29 Oct 2026. I need current, sourced facts, not opinions. Use live web search. For every claim, give a link and that page's date. Mark anything you couldn't open as **UNVERIFIED**. If you can't find something, write "not found". Don't guess.

## Q1. Which reviewers can I run on a public GitHub repo, today, for $0 or under $20 total?
I'll open about 30 seeded pull requests on two of my own public TypeScript repos and compare each tool's comments.

Cover CodeRabbit, Greptile, Cursor Bugbot, GitHub Copilot code review, Augment Code Review, Qodo Merge and Graphite Diamond. For each one, give:
- free tier or trial: does it cover public or open-source repos, and how many PRs or reviews;
- setup steps (GitHub App, account, card required?);
- whether it reviews PRs opened by the repo owner automatically, or needs a manual trigger;
- whether its terms of service allow publishing benchmark comparisons. Quote the clause in under 15 words, with the link.

Output a table, then a recommended set of 3–4 tools that fits $20.

## Q2. How do the top reviewers learn repo conventions and stop repeating rejected comments?
Cover CodeRabbit, Greptile, Cursor Bugbot, Copilot code review, Augment, Qodo and Graphite. For each one:
- **Conventions input:** file names and formats (for example `BUGBOT.md`, `.coderabbit.yaml`, `copilot-instructions.md`, AGENTS.md support), and what goes in them.
- **Feedback signals used:** 👍/👎, replies, resolved threads, whether the suggestion was applied, chat commands.
- **Mechanism,** as far as published: embeddings, rules extracted by an LLM, a per-team filter. Any published numbers on what it changed.

Output a table, then 5 bullets: which mechanisms an open-source GitHub Action could copy without a server, using only a git branch for storage.

## Q3. Cross-file breakage
- Does any AI reviewer publicly claim to detect breaks in files the PR **did not edit**, for example stale callers or renamed queue, route or env names? Quote the claim (under 15 words) with a link.
- Are there public benchmarks or papers (2023–2026) on cross-file or "semantic conflict" defects in code review or breaking-change detection? Give the title, link, year and one-sentence result.

## Q4. Two facts to check
- NVIDIA Nemotron 3 Super: what context length and recommended sampling settings does NVIDIA publish? Give the link.
- Nebius Token Factory: does it offer an embeddings model? If so, which ones and at what price? Give the link.

Keep the whole answer under 1,200 words, excluding tables.
