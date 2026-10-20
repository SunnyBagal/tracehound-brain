# grok-08 output (pasted by Sunny 20 Oct ~18:04 IST). Verbatim. Links survived this time. Chat check: research/R11-grok08.md

**Q1. Techniques that raise recall or quality without training (2024–2026 evidence)**

| Technique | One-line description | Source URL and date | Measured gain | Cost multiplier |
|-----------|----------------------|---------------------|---------------|-----------------|
| Specialized passes per category | Separate agents/prompts for security, bugs, tests, cross-file, etc. | https://blog.cloudflare.com/ai-code-review/ (2026-04-20); https://github.com/liatrio-labs/claude-code-gauntlet (2026-10-19) | Qualitative: more category coverage; Cloudflare reports volume by specialist (security 11k+ findings). No ReviewBench-exact % | ~4–7× if parallel (one call per specialist) |
| Per-file or per-hunk vs whole-diff | Label/review at hunk or file granularity then refine whole-patch | https://agentpatterns.ai/code-review/structure-aware-diff-labeling/ (2026-10-17, citing arxiv:2605.26100) | 84% recall / 81% precision on 95-hunk benchmark with Gemini-3-Pro (labeling task) | 1–2× (smaller contexts, possible multi-call) |
| Self-critique or reflection loops | Secondary LLM critiques/verifies first-pass findings | https://arxiv.org/abs/2601.09905 (2026-01-14); https://www.mdpi.com/2673-2688/7/9/353 (2026-09-07) | Mixed: +0.04–0.25 F1 on qualitative codes; often net negative on code gen (regressions > repairs) | 1.5–2× |
| Few-shot exemplars | Include similar past reviews or call-graph examples | https://arxiv.org/abs/2411.10129 (2024-11-15) | Function-call-graph few-shot on GPT-3.5 ~90% BLEU-4 lift vs pretrained baseline on CodeReviewer dataset | +10–30% tokens (examples) |
| “Think about the minimal fix” prompting | Require concrete input that breaks + smallest fix | https://howtoshipit.online/ai-code-review-prompts-patterns/ (2026-10-10); https://dev.to/novaelvaris/... (2026-03-27) | Qualitative: higher actionability; no published pure-recall delta on review benchmarks | ~1× (same call, longer output) |
| Multi-sample union vs voting | Generate N independent reviews, take union of findings (vs majority filter) | User’s own 5-run voting failed to raise F1; https://zenn.dev/sodeyama/articles/tech-ai-review-how-many-runs (2026-10-16) | Multi-runs continue to surface new true bugs (no early plateau); voting discards minority true positives | N× (N samples) |
| Reasoning-budget settings | Cap thinking tokens via enable_thinking / reasoning_budget | https://github.com/cobusgreyling/NVIDIA-Nemotron-3-Super/blob/main/budget-sweep/blog.md (2026-03); https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16 (2026-10-15) | Quality plateaus ~1024–2048 tokens for multi-step; higher budget adds latency/cost with diminishing return | 1.2–3× depending on budget |
| Retrieval of callers and callees | One-hop impact slice of changed-line callers/callees | https://opendatascience.com/context-engineering-for-ai-code-reviews-fix-critical-bugs-with-outside-diff-impact-slicing/ (2025-10-08) | Qualitative: catches contract/boundary bugs invisible in diff-only; wrong-file citations drop ~40%→0 with “CRITICAL” instruction | 1.2–1.5× (extra retrieval + context) |
| Running linters or type checkers as tools | Feed deterministic tool output (ESLint, CodeQL, etc.) into or alongside LLM | https://www.cubic.dev/blog/how-ai-code-review-works (2026-10-11); CodeRabbit docs (50+ tools) | Hybrid LLM+static cut 94–98% false positives while holding recall (Jan 2026 preprint on 433 alarms); noise reduction | ~1.1–1.3× (tool runtime + optional LLM verify) |

**Top 5 for recall per dollar (ranked):**
1. Specialized category passes (security / correctness / tests) — high recall lift per focused call.
2. Retrieval of callers/callees (impact slicing) — surfaces out-of-diff issues at low extra cost.
3. Multi-sample union (N=3) without voting filter — recovers variance-missed issues; voting already failed for you.
4. Per-hunk/file with local context + whole-patch refiner — better localization and lower context waste.
5. Moderate reasoning budget (1024–2048) + few-shot category exemplars — quality plateau without full N× cost.

**Q2. Learning from mistakes without weight updates**

Production systems convert feedback into future reviews via:
- **Memory of rejected comments**: Greptile suppresses patterns after consistent ignores/downvotes; claims 80% reduction in ignored comments (https://www.greptile.com/docs/how-greptile-works/memory-and-learning, 2026-09-01). Address rate rose 19%→>55% in two weeks (https://www.cubic.dev/blog/how-ai-code-review-works, 2026-10-11). CodeRabbit Learnings store chat-derived preferences; empirical study of 31k pairs shows 36.4% accepted / 56.3% rejected (https://arxiv.org/abs/2607.03316, 2026-07-03).
- **LLM-distilled per-repo rules**: Qodo Rules Miner extracts candidates from historical accepted PR comments (https://docs.qodo.ai/.../generate-and-manage-rules, 2026-05-18; https://www.qodo.ai/glossary/..., 2026-09-02). Cursor Bugbot uses `.cursor/BUGBOT.md` plus dashboard “learned rules” (https://cursor.com/help/ai-features/bugbot, accessed 2026). Graphite supports custom prompts and file-based rules plus exclusions (https://graphite.com/docs/ai-review-customization, 2026-02-02).
- **Retrieval of past accepted comments as few-shot**: Greptile and CodeRabbit surface repository patterns/history; few-shot with call graphs improved BLEU substantially in 2024 work (https://arxiv.org/abs/2411.10129).

**Open-source GitHub Action with git branch as storage (no server):** Yes for all three. Store distilled rules as Markdown/JSON in a branch (e.g. `.review/rules/*.md` or `rejected.json`). On each run, checkout the branch, retrieve top-k accepted comment snippets via simple similarity or keyword, inject as few-shot, and append new rejections/accepted patterns via a commit. Path-scoped files and suppression lists need no external DB.

**Q3. Light training options (feasibility check)**

- Nebius Token Factory fine-tuning/LoRA for Nemotron 3 Super or 3.5: Not listed. Supported models are DeepSeek, Meta Llama, Qwen, gpt-oss (https://docs.tokenfactory.nebius.com/fine-tuning/models — browsed 2026-10-20; no page date). Price and turnaround for Nemotron: not found. UNVERIFIED whether a later private offering exists.
- Nemotron 3.5 Super on Token Factory: The VL variant is available as `nvidia/Nemotron-3.5-Super-VL` at $0.30 input / $0.90 output per 1M tokens (https://tokenfactory.nebius.com/model-catalog.md and https://nebius.com/blog/..., 2026-10-16). No separate non-VL “3.5 Super” text-only id found. SWE-bench Verified 71.10, Multilingual 67.72, Pro 51.78 on the HF card (https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16, 2026-10-15). Nemotron 3 Super reported ~60.47% SWE-bench Verified (multiple 2026-03 sources).
- NVIDIA recommended sampling for Nemotron 3 / 3.5 Super: Text temperature 1.0, top_p 0.95; vision 0.6 / 0.95. Reasoning via chat-template `enable_thinking=True/False`; medium effort (`medium_effort` or equivalent) reduces tokens; `reasoning_budget` integer (e.g. 1024–8192) caps thinking (HF card 2026-10-15; Unsloth and NVIDIA cookbook notes).
- Small-scale RL / preference tuning (DPO/KTO) for code review comments: Not found for review-comment preference data. Closest: DPO-f+ aligns repair feedback (arXiv:2511.01043, 2025; +5.71 pp Pass@1 on novice tasks, +1.67–4.67 pp issue resolution on SWE-bench Lite). Target-DPO / CodeFlow for generation (EMNLP 2025). Data sizes typically tens of thousands of pairs; gains are on generation metrics, not ReviewBench-style grounded recall.

**Q4. Recommendation**

**3 changes most likely to raise ReviewBench grounded recall ≥5 points within 10 days and $40:**
1. Add 3–4 specialized generator passes (correctness/reliability, security, API-contract, tests) and pool by union before the existing verifier. Production systems (Cloudflare, Claude gauntlet) show category coverage gains; cost stays inside budget at Nemotron pricing if limited to changed hunks.
2. Strengthen context retrieval to explicit one-hop callers + callees of changed symbols (beyond git-grep names). Outside-diff impact slicing targets the boundary bugs that dominate golden issues.
3. Switch from single (or voted) generation to multi-sample union of 3 runs (temp 0 or low reasoning budget) with simple deduplication; keep the verifier. Your voting already failed; literature and multi-run measurements show union continues to recover additional true positives that majority filters discard.

**2 things that sound good but will not move the needle:**
- Pure multi-run voting or evidence-quote verification alone: you already measured zero F1 lift; voting systematically drops minority true positives.
- Unconstrained self-critique / reflection loops without an external tool or execution check: multiple 2024–2026 papers show self-critique can collapse performance or amplify self-bias on verification tasks; the extra call rarely yields net grounded-recall gains at this budget.