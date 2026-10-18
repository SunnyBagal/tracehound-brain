# R4 · How the top reviewers get precision (grok-05)

**Source:** Grok, 18 Oct 2026, prompt `prompts/2026-10-18/grok-05-precision-research.md`.

**Kill test (chat, 18 Oct):** spot-checked 3 links; all real and matching.
- Cursor "Building Bugbot" (15 Jan 2026). Its early pipeline is "eight parallel passes with randomized diff order → majority voting to drop single-pass bugs → validator model". It gives no precision number, only resolution rate 52 → 70%+.
- Augment benchmark post (11 Dec 2025, updated 18 Jun 2026). Augment reports 65% precision, 55% recall, F 59. The Context Engine is credited mainly for **recall**, and precision for "purpose-built review tuning".
- arXiv 2603.00539: overcorrection, plus a Fix-guided Verification Filter.

Other links were not opened; re-open any before quoting it publicly.

**Chat's reading:**
- **064's voting design has direct precedent:** Cursor's pre-launch Bugbot. We don't randomize diff order; Cursor did, for diversity.
- Next candidates if voting loses or plateaus:
  - (a) randomized hunk order and/or temperature 1.0 (NVIDIA's recommended sampling, R5) to make votes more independent;
  - (b) a per-PR cap / silence when unsure (Copilot);
  - (c) a cross-model critic (Greptile's "model inversion", arXiv 2610.01471). Hackathon rules favour NVIDIA models, so Nano or Super as the critic rather than another family.
- Augment's precision comes from tuning, not retrieval. That matters, because our ctx reviewer was a recall play too.

---

**Products**

| Product | Technique | Claim | Link | Date |
|---------|-----------|-------|------|------|
| Augment Code Review | Context Engine (semantic retrieval of relevant files, dependencies, call sites beyond the diff) + repository/hierarchical guidelines + system prompt that prioritizes high-signal issues over nits | Enables both higher precision and higher recall by letting the agent verify suggestions against real context instead of guessing; highest F-score (59% from 65% precision / 55% recall) on their 50-PR public benchmark among 7 tools | https://www.augmentcode.com/blog/we-benchmarked-7-ai-code-review-tools-on-real-world-prs-here-are-the-results | 2025-12-11 |
| Cursor Bugbot | Early: 8 parallel passes with randomized diff order + majority voting (drop single-pass bugs) + category filter + validator model. Later: fully agentic loop with tools and dynamic context | Newer versions catch more bugs without comparable rise in false positives; resolution rate rose from 52% to >70% across 40 experiments; resolved bugs per PR roughly doubled | https://cursor.com/blog/building-bugbot | 2026-01-15 |
| Greptile | Team-partitioned embedding filter (block comment if cosine-similar to ≥3 unique downvoted comments; pass if similar to ≥3 upvoted) + codebase indexer/wiki (docs, issues, past incidents) injected into agent + model inversion (review with different model family) | Address rate improved from 19% to >55% within two weeks of embedding filter; model inversion associated with ~10% higher bug catch rate | https://www.zenml.io/llmops-database/improving-ai-code-review-bot-comment-quality-through-vector-embeddings ; https://www.greptile.com/blog/how-to-build-a-code-review-agent | 2025 (ZenML case); 2026-10-09 |
| CodeRabbit | Specialized parallel agents including a Verification agent; sandboxed repo clone + static analyzers; independent verification of candidates (reachability, safeguards, evidence) before posting; deduplication/filtering | Verification step filters noise so posted comments are higher precision; security pipeline explicitly rejects candidates that fail independent verification | https://dev.to/izgorodin/coderabbit-vs-bugbot-2j6m ; https://www.coderabbit.ai/blog/security-agent-benchmarks | 2026-10-12 ; 2026-10-06 |
| GitHub Copilot code review | Ensemble of agents (Lite tier) whose findings are combined; broader shell tools (build/test/scripts) for validation; hybrid with deterministic tools (CodeQL/ESLint); skips low-value files | Ensemble increased addressed high-severity comments ~47%, medium ~31%; shell tools associated with more high-severity findings and fewer nits; silence preferred to noise (71% of reviews have actionable feedback) | https://github.blog/changelog/2026-09-11-auto-resolution-and-analysis-updates-in-copilot-code-review/ ; https://github.blog/ai-and-ml/github-copilot/60-million-copilot-code-reviews-and-counting/ | 2026-09-11 ; 2026-03-05 |
| Graphite Diamond | Context-aware analysis of broader diff + surrounding code; precision-focused design (favor actionable comments, trade coverage); continuous learning from developer feedback | AI feedback tends toward lower end of industry FP rates (often 5–8% vs 5–15% typical) | https://graphite.com/guides/ai-code-review-false-positives | Undated (accessed 2026) |
| Macroscope | Detection Mode dial (Prefer Precision vs Prefer Coverage) per repo; agentic exploration + “auto-tune” (LLM search over prompt/model/language combinations) | v3 raised overall precision from 75% to 98% on their benchmark while cutting comments 22% and increasing critical-bug detection; Prefer Precision posts only high-confidence findings | https://macroscope.com/blog/code-review-v3 ; https://macroscope.com/content/ai-code-review-precision-vs-recall | 2026-02-04 ; 2026-07-08 |
| Qodo | Multi-agent specialists (correctness, standards, architecture, risk) + judge agent that merges, deduplicates, and filters low-confidence/low-relevance findings; learns from team PR history | Judge keeps only high-confidence findings; multi-agent design cited as enabling competitive precision with higher recall (their benchmark F1 60.1%) | https://www.qodo.ai/blog/introducing-qodo-2-0-agentic-code-review/ ; https://docs.qodo.ai/code-review | 2026-02-04 |

**Papers (2024–2026)**

| Paper | arXiv / URL | Main result (one sentence) | Measured precision gain? |
|-------|-------------|----------------------------|--------------------------|
| Are LLMs Reliable Code Reviewers? Systematic Overcorrection… | https://arxiv.org/abs/2603.00539 | LLMs systematically misclassify correct code as defective (overcorrection bias); more detailed/explanation-requiring prompts increase the error rate; proposes Fix-guided Verification Filter that runs original vs proposed fix against tests | No measured production precision number; proposes filter |
| SGCR: A Specification-Grounded Framework for Trustworthy LLM Code Review | https://arxiv.org/abs/2512.17540 | Dual-pathway (explicit rule compliance + implicit discovery) grounded in human specs raised developer adoption of suggestions from 22% to 42% in industrial deployment | Yes (adoption rate as proxy for useful/precision-like signal) |
| Refute-or-Promote: Adversarial Stage-Gated Multi-Agent Review… | https://arxiv.org/abs/2604.19049 | Adversarial kill mandates + context asymmetry + cross-model critic + empirical gates filter LLM false positives in defect discovery; CMC killed additional candidates that survived earlier stages | Partial (describes kills of false positives; no single % precision delta) |
| Multi-Agent Code Verification via Information Theory | https://arxiv.org/abs/2511.16708 | Four specialized agents with low correlation catch more bugs than any single agent; multi-agent combination improved accuracy 39.7 pp vs single-agent baseline on labeled samples | Yes (accuracy / detection improvement reported) |
| When Does a Second Model Help? Cross-Model Review in LLM Verification | https://arxiv.org/abs/2610.01471 | Cross-model review finds partially different errors from same-model; one same + one cross matched more planted errors than two same-model reviews in one setting | Yes (coverage of planted errors) |
| CodeX-Verify style multi-agent (related) | (see 2511.16708 above) | Specialized agents improve detection while controlling false positives relative to single models | Yes |

**Quick wins (ranked by expected precision gain per dollar; testable in <1 day each)**

| Rank | Technique | Expected gain / dollar | Type | Source / note |
|------|-----------|------------------------|------|---------------|
| 1 | Multi-pass (3–8 samples, different order or temperature) + majority/agreement filter (drop single-pass findings) | High / low (parallel inference is cheap relative to agent loops) | Measured | Cursor Bugbot early design explicitly used this and filtered single-pass bugs (https://cursor.com/blog/building-bugbot) |
| 2 | Separate critic / verifier pass (different model or same with “prove it or drop it” prompt; require code citation or counter-example) | High / low–medium | Measured / strong precedent | CodeRabbit verification agent; Macroscope/Qodo judge; Cursor validator; arXiv:2603.00539 fix-guided idea; cubic summary of industry practice |
| 3 | Severity / confidence gating + hard comment cap per PR (post only top-k high-severity) | Medium–high / near-zero | Measured | GitHub Copilot reports silence on 29% of reviews and focuses on high-signal; Macroscope Prefer Precision; industry pattern of dropping low-confidence |
| 4 | Skip low-value files + comment only on changed lines (ignore lockfiles, generated, pure style, pre-existing issues) | Medium / near-zero | Measured | Copilot skips manifests/logs/SVGs; Google AutoCommenter dropped comments on untouched lines (80% of raw output in one report); common industry filter |
| 5 | Per-team feedback filter (simple embedding or keyword of past downvotes/ignores; block similar new comments) | Medium / low | Measured | Greptile embedding filter raised address rate 19% → >55% in two weeks (ZenML case) |
| 6 | Hybrid static + LLM (run linters/CodeQL first; only let LLM comment on or expand deterministic findings, or require LLM to cite a tool result) | Medium / low | Measured | GitHub Copilot hybrid with CodeQL/ESLint; CodeRabbit runs 50+ analyzers then verifies |
| 7 | Model inversion / cross-family review on the final candidates | Low–medium / low | Measured | Greptile reports ~10% higher bug catch (and reduced shared blind spots) |

**Opinion:**

Context retrieval (Augment-style) and verification-before-post (CodeRabbit/Macroscope/Qodo/Cursor) are the two techniques most consistently credited for moving precision from the 30–50% band into the 60–80%+ band without collapsing recall.  
Self-consistency via multi-pass voting is the cheapest immediate experiment and has public precedent from Cursor’s early Bugbot.  
A lightweight critic pass that demands executable or cited evidence will likely give the largest precision lift per extra token on Nemotron.  
Learned per-repo filters (embeddings or simple rules from thumbs-down) compound over days and are almost free after the first week of data.  
The dominant remaining gap is not model quality but post-generation gating; most high-precision products are stricter about what they allow to reach the PR.