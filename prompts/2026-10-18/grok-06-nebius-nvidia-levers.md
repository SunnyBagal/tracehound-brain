# grok-06: Nebius, NVIDIA and hackathon levers we may be missing (research)

Tool: **Grok Premium, a fresh chat.** Paste everything below the line. When it's done, paste the answer back into the planning chat (Claude).

---

You are a research assistant. My project TraceHound is in the **Nebius × NVIDIA hackathon** (deadline 30 Oct 2026). It runs NVIDIA Nemotron 3 Super (`nvidia/nemotron-3-super-120b-a12b`) and Nemotron 3 Nano through **Nebius AI Studio / Token Factory's OpenAI-compatible API**. It is an AI pull-request reviewer, delivered as a GitHub Action.

Find, with links and dates (2026 preferred):

1. **The hackathon itself:** the official page, judging criteria and weights, required deliverables (video length, repo, write-up), prize categories, any "best use of Nemotron" or Nebius-specific prizes, and any public FAQ or office-hours notes. Also any past Nebius or NVIDIA hackathon winners, and what their submissions emphasised.
2. **Nebius Token Factory features we might be missing:**
   - a batch API or discounted async inference, with its price vs real-time;
   - prompt or prefix caching;
   - fine-tuning or LoRA hosting for Nemotron;
   - rate limits;
   - structured output or JSON mode;
   - per-model prices for Nemotron 3 Super and Nano.
3. **Nemotron 3 features:**
   - reasoning on/off and reasoning-budget control;
   - tool or function calling;
   - context length;
   - recommended sampling settings;
   - NVIDIA's own guidance for code review, agents or verification.
   Include any NVIDIA blog, model card or NIM docs.
4. **Anything showing NVIDIA or Nebius staff praising specific kinds of entries** (open benchmarks, honest evals, agents, developer tools).

Rules:
- Every fact needs a working link. Say "not found" rather than guess. One invented citation discards the whole answer.
- Verify every @handle you mention actually exists.
- Output one section per numbered item, each fact with its link, then "Opinion:" lines on which 3 levers matter most for a code-review entry.
