# R11 · grok-08 checked against R10 (chat, 20 Oct 18:20 IST)

**Source:** Grok, 20 Oct 2026, prompt `prompts/2026-10-19/grok-08-smarter-reviewer.md`. Raw output: `grok-08-smarter-reviewer.out.md`. ChatGPT's answer to the same prompt is still to come; it gets its own section here.

## Kill test (P003): 5 of 5 links real, 3 of 5 overstated

| Link | Grok's claim | What the page says |
|---|---|---|
| [zenn.dev/sodeyama … how-many-runs](https://zenn.dev/sodeyama/articles/tech-ai-review-how-many-runs) (16 Oct 2026) | union of runs keeps finding new true bugs; voting drops minority TPs | **Partly true.** Recall of the union rises with runs and doesn't plateau (DeepSeek no-thinking: 2.2% at 1 run → 8.1% at 5 → 18.8% at 50; Qwen: thinking on 6.6% vs off 1.9% per run). Local models, 26 PRs, 32 bugs, prompts tuned on the same bugs. **It never compares union vs voting.** |
| [ODSC impact slicing](https://opendatascience.com/context-engineering-for-ai-code-reviews-fix-critical-bugs-with-outside-diff-impact-slicing/) (8 Oct 2025) | callers/callees catch boundary bugs; wrong-file citations ~40% → 0 | **Anecdote.** One example; no recall numbers; the 40% → 0 has no sample or method. Speaker is CodeRabbit's Director of AI. Grok's Q4 line "boundary bugs that dominate golden issues" has no source. |
| [HF Nemotron 3.5 Super VL card](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16) (15 Oct 2026) | SWE-bench V 71.10; text 1.0/0.95; `enable_thinking`, `medium_effort`, `reasoning_budget` | **True except `reasoning_budget`**: the card (first 100k chars) documents off / on (medium) / on, via `chat_template_kwargs`, and no `reasoning_budget` param. Reasoning tokens share `max_tokens`. |
| [Token Factory fine-tuning models](https://docs.tokenfactory.nebius.com/fine-tuning/models) | no Nemotron | **True.** DeepSeek, Llama, gpt-oss, Qwen3; no DPO/RL. Matches R10. |
| [Greptile memory & learning](https://www.greptile.com/docs/how-greptile-works/memory-and-learning) | 80% fewer ignored comments (dated 2026-09-01) | **Number on the page, but a vendor claim** with no method or sample; the page has **no date** (Grok's date is invented). |

Grok passes P003 (nothing fabricated), but its dates and its Q4 framing are looser than its links.

## Agreements with R10
- No Nemotron fine-tuning or DPO on Token Factory. Settled twice now.
- 3.5 VL id `nvidia/Nemotron-3.5-Super-VL`, $0.30/$0.90, SWE-bench V 71.10 vs ~60.5. **Still needs Sunny's console check.**
- NVIDIA text sampling 1.0 / 0.95; we run 0. Stays a later variable (R8, R10).
- Learning patterns (Greptile suppression, Bugbot rules, Qodo miner) = codex-09 now, learned rules later. None moves a benchmark.
- Voting and evidence-quote verification don't help (our 064, 065).
- Unconstrained self-critique: skip.

## Disagreements
1. **Specialist category passes (Grok's #1).** Grok's evidence is Cloudflare finding *volumes* and a repo, no measured gain. R10's RevAgent evidence says the gain came from per-agent LoRA. **R10 holds.** Also out of scope for P008: new prompts.
2. **Caller/callee slicing (Grok's #2).** Anecdote only. Our pipeline already pulls repo context by `git grep` of changed names, and contracts (#47) cover the typed cross-file case. **Not a ReviewBench lever** (new code, P008 forbids it). Possible v2 item after submission.
3. **Multi-sample union (Grok's #3).** Agrees with us: it's **already pre-registered** as `pool-raw-x2`. zenn supports it (recall keeps rising with N). No change.
4. **Reasoning budget 1024–2048.** Sourced to a community blog for 3 Super; not on the 3.5 card. We run reasoning **on** (model default, D034/benchmark.md). zenn's thinking-on vs off gap (6.6 vs 1.9%) argues we keep it on. No change.

## P008 amendment?
**No.** No ReviewBench test run has happened, so one was allowed, but Grok surfaced nothing that both (a) R10 missed and (b) runs on existing frozen prompts. Its top 3 are either already candidate 3 (`pool-raw-x2`) or need new prompts/code. Adding more candidates on 25 PRs only adds selection bias.

## One thing to carry into rb-01b
- Reasoning tokens share `max_tokens` on 3.5. If our stage calls cap `max_tokens` low, 3.5 could truncate answers. rb-01b's 3.5 smoke run must report **finish_reason** and whether any stage returned empty/truncated content.
