# grok-07 output (Grok, pasted by Sunny 19 Oct 12:27). Links were lost in the paste; checked by chat in research/R8.

**Q1. Reviewers runnable on public GitHub repos today for $0 or ≤$20 total (30 seeded PRs on two TypeScript repos).**

| Tool | Free tier/trial for public/OSS | Setup | Auto-review owner PRs? | ToS on publishing benchmarks |
|------|--------------------------------|-------|------------------------|------------------------------|
| **CodeRabbit** | OSS plan: unlimited public repos, Team features, rate limits 1–10 reviews/dev/hour (varies by popularity); no card. Free plan: summaries only.  | GitHub App install; account signup. No card for OSS/Free. | Yes, automatic on eligible PRs to default branch (drafts skipped); manual `@coderabbitai review`.  | Prohibits: “disclose the results of any benchmarking of the Services” (ToS §3, updated Feb 2, 2026).  |
| **Greptile** | Starter: free for 1 active developer, 50 credits/month (Base=1 credit). OSS program: free for qualified non-commercial OSI-licensed projects (apply). 14-day Pro trial.  | Sign in at app.greptile.com, install GitHub App on repos. | Yes, automatic on PRs (skips drafts by default); manual `@greptileai`. | Not found (ToS restricts unauthorized third-party access/results sharing, no explicit benchmark ban). UNVERIFIED for exact clause. |
| **Cursor Bugbot** | Usage-based (~$1.00–$1.50 avg/run); Individuals draw from plan included usage then on-demand. 14-day trial. No permanent free unlimited.  | Cursor account + GitHub connection/dashboard enable. Card/usage for paid. | Yes, automatic on PR updates by default; can set manual/mention-only.  | Not found. |
| **GitHub Copilot code review** | Not on Free (only selection review in VS Code). On Student/Pro+ (Pro $10/mo, 1000–1500 credits; free Pro for some OSS maintainers/teachers). Public repos no Actions minutes. Credits ~$0.05–$5/review by effort.  | GitHub account + Copilot plan; request as reviewer or enable auto. | Manual request by default; auto if author/ruleset enables. | Not found. |
| **Augment Code Review** | Free for OSS projects (apply with link). Paid plans include credits; Indie ~$20/mo. One-time credits reported in some free starts.  | Account + GitHub App/connect in settings. | Configurable; reviews enabled repos. | Not found. |
| **Qodo Merge** | Free Developer: ~30–75 PR feedbacks/org/month. OSS program: free for public repos with 200+ stars (or org has one); core features, no caps in some announcements. Self-host PR-Agent free (own LLM keys).  | GitHub Marketplace App install. | Auto on PRs or `/review`/`/improve`. | Not found. |
| **Graphite Diamond/Agent** | Hobby (free): personal-account repos, limited AI reviews (number unpublished). Starter $20/user/mo (annual, limited). 30-day Team trial, no card.  | Graphite account + GitHub. | Yes once enabled for repo. | Not found. |

**Recommended set for ≤$20 (fits ~30 PRs):** CodeRabbit OSS (free, rate-limited but sufficient for seeded tests), Greptile Starter (50 credits cover 50 Base) or OSS apply, Qodo OSS if stars qualify (or free quota), GitHub Copilot Pro if free via maintainer status or $10 seat (credits for public). Avoid Bugbot for volume (usage exceeds $20 quickly). Total near $0–$10. Sources checked Oct 2026; confirm live pages. 

**Q2. Learning repo conventions and avoiding repeated rejected comments.**

| Tool | Conventions input | Feedback signals | Mechanism (published) |
|------|-------------------|------------------|-----------------------|
| **CodeRabbit** | `.coderabbit.yaml` (instructions, path_instructions, profile); auto-detects AGENTS.md, CLAUDE.md, .github/copilot-instructions.md, .cursorrules, etc.  | Replies (`@coderabbitai`), resolved threads, chat. | Rules from config + guidelines files; learns from feedback over time (docs imply adaptation). No specific published numbers. |
| **Greptile** | Custom rules (org-wide, `.greptile/` or greptile.json); plain-English guidelines.  | 👍/👎 reactions; comments. Learns over 2–3 weeks. | Full-repo graph/embeddings for context; rules + reaction-based tuning. Claims better cross-file. |
| **Cursor Bugbot** | `.cursor/BUGBOT.md` (rules, tone, ignore); Team Rules.  | Reads PR comments (top-level/inline); resolution implied. | Rules file + comment context; optimizes for fix rate (80% resolution published). |
| **Copilot code review** | `.github/copilot-instructions.md`; path-specific `*.instructions.md` (applyTo); AGENTS.md, etc.  | Limited; replies not fully used by model. | Instruction files injected into prompt; no strong published adaptive filter. |
| **Augment** | Custom rules for team practices. | Adaptive learning from addressed comments. | Context Engine (full-repo retrieval); learns from feedback. Claims high precision.  |
| **Qodo** | Rules/standards (paid/advanced); OSS core limited. | Not detailed publicly for free. | Rules engine; multi-agent in paid. |
| **Graphite Agent** | Customizable rules/filters. | Learns from comment feedback. | Codebase-aware; feedback-based adaptation. |

**5 mechanisms an open-source GitHub Action could copy (git branch storage only, no server):**
- Parse and inject convention files (AGENTS.md, .coderabbit.yaml-style, BUGBOT.md, copilot-instructions.md) into prompt.
- Store reaction/reply history as JSON in a dedicated branch; filter future comments matching rejected patterns via simple LLM or rules.
- Use resolved-thread status from PR API to suppress similar suggestions.
- Path-scoped rules from config committed to repo.
- Comment-history embeddings or keyword filters computed locally and stored as files in branch (no external index).

**Q3. Cross-file breakage.**
- Yes. Greptile: “catch complex bugs that are not in the diff but second order effects of the diff” via codebase graph.  Augment: “catches correctness, architectural, and cross-system issues” with full-repo context.  Others (CodeRabbit multi-repo analysis limited by plan) do not prominently claim unedited-file breaks.
- Benchmarks/papers: ContextCRBench (2025, arXiv-related; context-enriched fine-grained review; textual context > code context, LLMs far from human).  ReviewBench (GitHub, Oct 2026; representative PRs, multi-source golden set).  SAM: Detecting semantic conflicts with unit tests (2024; test-generation detects 9/29 conflicts).  AgenticFlict (2026; ~27.7% merge conflict rate in agent PRs).  Semantic conflict static analysis comparison (2025). Not found: direct “stale callers/renamed queues” AI-reviewer benchmarks.

**Q4. Facts.**
- NVIDIA Nemotron 3 Super: Context up to 1M tokens (default often 256K/262144). Recommended sampling: temperature=1.0, top_p=1.0 (general) or top_p=0.95 (all tasks in some NIM docs); tool-calling lower.  
- Nebius Token Factory: Yes, embeddings. Examples: Qwen3-Embedding-8B ($0.01/1M input, 4096 dims); BAAI bge-multilingual-gemma2 / BGE-ICL (~$0.01/1M); intfloat/e5-mistral-7b-instruct ($0.01/1M).  

All claims sourced from pages accessed ~19 Oct 2026; mark UNVERIFIED where page not fully opened or details sparse. Not found where no public source located.