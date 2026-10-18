# R5 · Hackathon rules, Nebius and Nemotron levers (grok-06)

**Source:** Grok, 18 Oct 2026, prompt `prompts/2026-10-18/grok-06-nebius-nvidia-levers.md`.

**Kill test (chat, 18 Oct):** Devpost page opened.
- Deadline **30 Oct 10:00 PDT = 22:30 IST** ✓.
- Four criteria ✓ (Technological Implementation, Design, Potential Impact, Quality of the Idea). **Grok's "25% each" is NOT on the page**; treat weights as unknown.
- Deliverables ✓:
  - a demo URL;
  - YouTube video ≤ 3 min;
  - a public repo with the licence visible plus a README with setup;
  - a feedback section;
  - an explanation of significant updates if the project predates 26 Aug.
- Tracks and prizes ✓.

**Not verified:**
- Batch API at 50% of the real-time price (a search didn't confirm it);
- the $25 credit code;
- the judges' names;
- the Nemotron `reasoning_budget` parameter;
- the "temperature 1.0" recommendation.

**Chat's reading, actions:**
1. **Track: Coding and Agentic Engineering.** Its stated preference is "an agent that plans, writes, tests and iterates on a real repo". Our one-shot review plus fix suggestion is weaker on that axis. The repo already has a sandboxed repair harness (FixFlow era). An optional "verified fix" path (fix → run the tests in the sandbox → post only if green) would match the track. Cost: days. Decide after 064.
2. **Feedback section is a required deliverable** and has its own prize (Most Valuable Feedback, $100 × 10). FEEDBACK.md is already detailed, so polish it on 26 Oct.
3. **"Significant updates since 26 Aug"** is required, so the open question about FixFlow history must be answered.
4. A demo URL is required: the Vercel viewer plus the dashboard (ui-02).
5. Credits: check whether the Devpost $25 code and the Builders Program $25 were already redeemed.

---

**1. The hackathon itself**

Official page: [Nebius x NVIDIA Global AI Hackathon on Devpost](https://nebiusglobalaihackathon.devpost.com/). Deadline is 30 Oct 2026 at 10:00 AM PT / 1:00 PM EDT (submission period started 26 Aug 2026 9:00 AM PT).

There is a related one-day in-person event (Nebius Build Week Hackathon, Berlin, 18 Oct 2026) focused on the same stack; the main prize event is the online global one above.

Judging criteria (Stage 1 pass/fail viability + theme fit; Stage 2 equally weighted, each 25%): Technological Implementation (effective use of Nebius Token Factory/AI Cloud + NVIDIA model), Design (complete coherent product, not just a POC), Potential Impact (credible real problem for a real audience, demonstrated), Quality of the Idea (creative/non-obvious use + genuine understanding). Tie-break order starts with Technological Implementation. Judges specifically prefer tracks pushed beyond basics (e.g., Coding & Agentic Engineering: an agent that plans/writes/tests/iterates on a real repo with minimal human input, not a simple completion wrapper).

Required deliverables: working project on Nebius Token Factory or AI Cloud using ≥1 NVIDIA open-source model (Nemotron preferred in Best Apps & Agents); public open-source repo (GitHub/GitLab/Bitbucket, license visible, README with setup + clear callouts of Nebius + model use); public YouTube demo video ≤3 minutes (treat as pitch: problem → working solution → who it’s for → how it uses Nebius/NVIDIA; name tools with audio); demo URL (except Physical AI); project description; required feedback section on the tools/models used (what worked, what needs work, onboarding, would you build again). Pre-existing projects need a written explanation of significant updates during the period. Hide API keys.

Prize categories (total pool $50,000+): Grand $20,000, 2nd $10,000, 3rd $6,000 (overall); each of 4 tracks (Coding and Agentic Engineering, Best Apps and Agents, Personal AI, Physical AI) winner gets an NVIDIA Jetson Orin Nano; Best Use of Tavily $3,000; City Winner Awards $500 × 20; Most Valuable Feedback $100 + NVIDIA swag × 10. One overall or one track + one bonus max. No dedicated “best use of Nemotron” or Nebius-only cash prize beyond the track/overall structure and model-use requirements.

Tracks (summary): Coding and Agentic Engineering (coding agents/developer tools that write/run/test code); Best Apps and Agents (usable apps/agents, Nemotron Ultra for hard reasoning + Nano/Super for fast calls; Serverless Endpoints/Jobs encouraged); Personal AI (persistent memory + tools); Physical AI (embodied/edge, ≥1 min hardware or module footage).

Public FAQ: updates page points to a new FAQs page and directs questions to Nebius Discord; resources page covers $25 Token Factory credits (code NEBIUS-DEVPOST-GLOBAL26) + another $25 via Nebius Builders Program (plus Tavily/Academy credits and office hours). No detailed public office-hours notes found beyond the credits/resources mention and Berlin event mentoring.

Past Nebius/NVIDIA hackathon winners: NVIDIA’s Nemotron Developer Days Seoul 2026 (Apr 21–22) — Nota AI won Grand Prize + Track C (synthetic data generation) by emphasizing synthetic data pipelines, MoE-specific quantization (NMQ), potential NVIDIA Model Opt collaboration, and integration into their NetsPresso platform rather than pure research. Nebius Robotics & Physical AI Awards 2025 winners (credits, not this hackathon) emphasized data engines/simulation, foundation models/runtime, industrial deployment, and vision analytics.

**2. Nebius Token Factory features**

Batch / discounted async inference: Batch API exists (OpenAI-compatible style, JSONL upload, async processing, up to 10 GB / millions of requests per file, concurrent batches). Pricing page states batch inference is automatically billed at 50% of the base real-time model price (rounded up to nearest cent). Rate-limits docs explicitly recommend Batch API for async workloads because it has significantly higher limits. Example comparison for 1M requests is shown on the batch page.

Prompt / prefix caching: Observability metrics include KV-cache hit rate. Responses API supports a `prompt_cache_key` parameter. A public feature-request thread (Mar 2026) asked for implicit/explicit discounted prompt caching because agentic/tool-use workflows re-send context; no confirmed public discounted cached-input pricing for all models was found in the docs reviewed (some third-party tables list cached prices for certain NVIDIA models, but not verified on Nebius pricing pages).

Fine-tuning / LoRA hosting for Nemotron: Fine-tuning (LoRA and full) is supported for listed families (Llama, Qwen, DeepSeek, GPT-OSS, etc.; default context 8k, configurable higher). The model catalog tags several Nemotron variants (including Nemotron-3-Super-120b-a12b and Nemotron-3-Nano-30B-A3B) with “Public Dedicated Fine-tuning,” but the explicit “models for fine-tuning” docs page does not list Nemotron among the supported base models. Dedicated endpoints support custom weights for eligible models. “Not found” as a confirmed public serverless LoRA path specifically for Nemotron 3 Super/Nano in the primary fine-tuning model list.

Rate limits: Dynamic, auto-scaling in 15-minute windows (scale-up ×1.2 if avg usage ≥80%; scale-down ÷1.5 if ≤50%; hard ceiling 20× base). Baseline example shown: 60 RPM / 400k TPM. Headers expose remaining, reset, scale factor, over-limit processing. Enterprise removes caps. Batch has higher limits.

Structured output / JSON mode: Supported via `response_format` (`json_object` or `json_schema` with JSON Schema). Models tagged “JSON mode” on their cards are preferred; always reinforce schema in the prompt. OpenAI-compatible.

Per-model prices (public endpoints, approximate, base flavor, per 1M tokens, as listed on Token Factory endpoints/catalog around Oct 2026):  
- Nemotron-3-Super-120b-a12b (`nvidia/nemotron-3-super-120b-a12b`): $0.30 input / $0.90 output (~127 tok/s).  
- Nemotron-3-Nano-30B-A3B: $0.06 input / $0.24 output (~60 tok/s).  
(Context windows listed around 256k–262k on the catalog; model cards claim up to 1M native for Super.) Prices can vary by flavor/region; check live catalog.

**3. Nemotron 3 features**

Reasoning on/off and budget control: Configurable via chat template / `extra_body` (`enable_thinking=True/False`, `low_effort=True`, and integer `reasoning_budget` that caps thinking tokens). Super supports full, low-effort, and off modes. Docs and community demos show the pattern; NVIDIA NIM model card confirms it. Recommended as a way to trade latency vs depth.

Tool / function calling: Supported (OpenAI-compatible). Optimized for agentic/tool-use; Latent MoE specialization helps with tool calls, code, data analysis. Trained with RL on multi-environment agent tasks including tool use.

Context length: Native up to 1M tokens claimed for Super (hybrid Mamba-Transformer + Latent MoE + MTP). Catalog listings show 256k–262k for the Token Factory deployments of Super/Nano; treat the 1M figure as the model’s architectural maximum and verify the exact endpoint.

Recommended sampling: NVIDIA guidance (NIM model card and Unsloth/NVIDIA docs): temperature=1.0 and top_p=0.95 for all tasks (reasoning, tool calling, general). Some tool-calling notes suggest 0.6 / 0.95. Default reasoning is on.

NVIDIA’s own guidance for code review / agents / verification: Super is positioned for multi-agent systems, long-horizon planning, coding, software-engineering agents, and “Super + Nano” patterns (Super for complex reasoning, Nano for fast steps). Blog and model card highlight tool calling reliability, multi-step workflows, coding, and agent benchmarks (e.g., PinchBench). No dedicated “code-review best-practices” whitepaper found beyond the agentic/coding positioning and RL training on code/math/tool environments. Key NVIDIA sources: [Introducing Nemotron 3 Super (NVIDIA Technical Blog, 11 Mar 2026)](https://developer.nvidia.com/blog/introducing-nemotron-3-super-an-open-hybrid-mamba-transformer-moe-for-agentic-reasoning/), [NVIDIA NIM model card](https://docs.api.nvidia.com/nim/reference/nvidia-nemotron-3-super-120b-a12b), [NVIDIA Blog](https://blogs.nvidia.com/blog/nemotron-3-super-agentic-ai/).

**4. NVIDIA / Nebius staff praise for specific entry styles**

Judging update (Nebius) explicitly praises: agents that plan/write/test/iterate on real repos with minimal human input (Coding track); multi-step autonomous end-to-end workflows (Apps & Agents); persistent-memory assistants that take real action (Personal AI); live hardware reaction (Physical AI). Feedback quality is a required, scored element. Overall criteria reward effective use of the stack + complete product + real impact + non-obvious idea. No public quotes from named NVIDIA/Nebius staff specifically praising “open benchmarks” or “honest evals” for this hackathon were found; the closest is the emphasis on demonstrated working systems and honest tool feedback. Judges list includes NVIDIA and Nebius engineers (e.g., Nancy Agarwal, Abhinav Balasubramanian at NVIDIA; several Nebius Token Factory / Physical AI leads). @nebiusai and @nebiustf exist and have posted about the event and judges.

Opinion:  
The three highest-leverage moves for a code-review GitHub Action entry are: (1) lean hard into the Coding and Agentic Engineering track’s preferred pattern—an agent that actually plans, edits, runs tests, and iterates on a real repo with minimal human input, not a one-shot comment generator; (2) make Technological Implementation obvious and measurable (clear Super vs Nano routing, structured output for review findings, Batch or Serverless for any bulk analysis, and a crisp feedback section); (3) treat the ≤3-minute video as a pitch that shows the working Action on a real PR, names Token Factory + the exact Nemotron models with audio, and states the problem, audience, and impact—Design and Potential Impact are equally weighted with tech.