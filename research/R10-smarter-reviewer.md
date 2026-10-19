# R10 · What would make the reviewer smarter (chat research, 19 Oct 18:45–19:15 IST)

Done by the planning chat itself, because Sunny was away. This is the grok-08 question set, answered from primary pages where possible. Grok or ChatGPT can still run grok-08 as a second opinion.

## 1. The biggest lever: Nemotron 3.5 Super is on Token Factory at our current price
- [Nebius Nemotron models page](https://nebius.com/services/token-factory/models/nvidia-nemotron-models-inference) lists **NVIDIA Nemotron 3.5 Super VL** at **$0.30 / $0.90 per 1M tokens**, the same as Nemotron 3 Super in `config/prices.json`. The model id inferred from the page links is `nvidia/Nemotron-3.5-Super-VL`; **Sunny should confirm it in the console.** The same page lists Nemotron-3-Ultra-550b ($1/$3), 3.5 Lightning and 3 Nano.
- NVIDIA released it 14–15 Oct 2026 ([NVIDIA research page](https://research.nvidia.com/labs/nemotron/Nemotron-3.5-Super-and-Lightning/)). It's the 3 Super backbone plus a vision encoder, so text is a first-class use. Published scores, from different harnesses and versions, so read them as indicative:

| | Nemotron 3 Super | Nemotron 3.5 Super (VL) |
|---|---|---|
| SWE-bench Verified | 60.47 (OpenHands) ([NVIDIA build card](https://build.nvidia.com/nvidia/nemotron-3-super-120b-a12b.md)) | **71.10** ([aitoolsreview summary of the model card](https://aitoolsreview.co.uk/insights/nvidia-nemotron-3-5-super-vl)) |
| LiveCodeBench | 81.19 (v5) | 88.22 (v6) |
| Terminal-Bench | 31.0 (Core 2.0) | 60.67 (2.1) |
| GPQA-Diamond | 79.23 | 89.52 |

- Recommended text sampling: temperature 1.0, top_p 0.95. Reasoning is toggled with `enable_thinking` in the chat template; our client already passes `chat_template_kwargs`.
- **No code change is needed to try it.** Every stage (`review`, `review-ctx`, `verify-ctx`) takes `--model`. Temperature is hard-coded to 0 in `review.ts:155`, `review-ctx.ts:70` and `verify-ctx.ts:122`, so a first test is a **clean one-variable swap at temperature 0**. Price entry: add `nvidia/Nemotron-3.5-Super-VL` at 0.30/0.90 to `config/prices.json`; otherwise the budget guard uses the $5/$15 placeholder.

## 2. Training or RL: not feasible
- Token Factory post-training ([models list](https://docs.tokenfactory.nebius.com/post-training/models.md)) offers LoRA and full fine-tuning for Qwen, Llama, gpt-oss, DeepSeek and Gemma, with **no Nemotron**, and no DPO or RL listed. Fine-tuning another family would drop the Nemotron story the hackathon wants.
- RevAgent ([arXiv 2511.00517](https://arxiv.org/html/2511.00517v1)): five category-specific commentators plus a critic. Its gains (BLEU +12.9%, ROUGE-L +10.9%) come mostly from **per-agent LoRA fine-tuning**. Removing the fine-tuning caused the biggest drop, and it costs about 10× the tokens. So the "specialist passes" evidence does not carry over to a prompt-only setup.

## 3. Learning from mistakes: how the leaders do it (no weight updates)
- **Cursor Bugbot learned rules** ([cursor.com/blog/bugbot-learning](https://cursor.com/blog/bugbot-learning), 8 Apr 2026):
  - Signals: reactions, replies, and human comments on bugs Bugbot missed.
  - Mechanism: signals become **candidate rules**; a rule activates once enough signal accumulates and is disabled after consistent negative signal; users can edit rules.
  - Reported resolution rate: 52% → ~78%, with 44k learned rules across 110k repos (Cursor's own LLM-judged figures).
- **Greptile** ([ZenML case study](https://www.zenml.io/llmops-database/improving-ai-code-review-bot-comment-quality-through-vector-embeddings)):
  - Mechanism: per-team embeddings of up- and down-voted comments; a new comment is blocked if it's similar to ≥3 downvoted ones.
  - Result: address rate 19% → 55%+.
  - What failed first: prompting and few-shot ("made results worse") and an LLM severity judge ("nearly random"). **That matches our own 056/065 losses.**
- **For us:** codex-09 is the Greptile pattern, done lexically, with Nebius embeddings as an option. The Bugbot pattern (candidate → active rules distilled from feedback, injected into the prompt) is the natural next step, after the hackathon or as a Claude B job. **Neither can move a benchmark number**, because benchmark PRs carry no feedback. They are product value, shown in a demo.

## 4. Recommendation
1. **Add Nemotron 3.5 Super to the ReviewBench candidates** before any run (P008 amendment), and make rb-01 support it (`RB_CONFIG_MODEL`, a price entry, a smoke run on one PR with 3.5).
2. If 3.5 wins the 25-PR test runs, run a **3-run dev check** (about $5.4, pre-registered) before switching the Action's default model, so the product and the benchmark stay the same reviewer.
3. Temperature 1.0 / top_p 0.95 is a separate, later variable, and it needs a small code change (a temperature option per stage).
4. Don't do now: specialist passes (no evidence without fine-tuning), an LLM severity filter (Greptile says random; our 065 agrees), or fine-tuning (no Nemotron support).
