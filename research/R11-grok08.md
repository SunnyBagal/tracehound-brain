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

---

# ChatGPT pass on the same prompt (chat check, 20 Oct 18:35 IST)

Raw output: `prompts/2026-10-19/grok-08-smarter-reviewer.chatgpt.out.md`. Better than Grok: it flags its own gaps ("not found", "bundled", "estimate") and doesn't claim any single change gives ≥5 points.

## Kill test (P003): 5 of 5 real; 1 misattribution, 1 number not in the text

| Link | ChatGPT's claim | What the page says |
|---|---|---|
| [CR-Bench, arXiv 2603.11078](https://arxiv.org/html/2603.11078v1) (10 Mar 2026) | missed-issue reflection: recall 27.01 → 32.76 (GPT-5.2), 18.39 → 27.59 (GPT-5-mini); usefulness 83.6 → 66.1 | **True.** 174 PRs from SWE-bench, Sonnet 4.5 judge. But the loop is **iterative with an unstated number of calls**, closed models only, precision stays ~3–5%, SNR falls 5.1 → 2.0. |
| [Qodo benchmark post](https://www.qodo.ai/blog/qodo-outperforms-claude-in-code-review-benchmark/) (12 Mar 2026) | extended mode recall 60 → 71, F1 68.2 → 74.7 | **Partly checkable.** Default 79 P / 60 R is in the text; the extended numbers are **only in charts**. Qodo's own benchmark (injected bugs). "Extended" = category agents **plus models from OpenAI, Anthropic and Google** plus verification: bundled, not prompt-only on one model. |
| [LAURA, arXiv 2512.01356](https://arxiv.org/html/2512.01356v1) | context +4.7, exemplars +4.4; cited for callers/callees | Numbers right (human "helpful/instrumental" share, 384 samples, GPT-4o). **Misattributed:** LAURA's context is the enclosing function + imports + PR text, **not callers/callees**, and the metric isn't recall. |
| [OpenCodeReview, arXiv 2608.09290](https://arxiv.org/html/2608.09290v1) (10 Aug 2026) | F1 11.57 → 25.10 while recall 28.9 → 20.0 | **True.** AACR-Bench, 200 PRs, Qwen3-235B judge. Gain is all precision; recall **fell**. No per-file ablation. |
| [Cursor: building Bugbot](https://cursor.com/blog/building-bugbot) (15 Jan 2026) | 8 passes + majority vote, voting's gain not isolated | **True.** Also: resolution 52% → 70%+ over 11 versions; the **largest gain came from moving to a fully agentic design**. |

## What's new versus R10 and Grok
1. **Missed-issue pass (CR-Bench).** The only controlled result for a prompt-only change that raises recall by ≥5 points. R10 and Grok both missed it. It suits ReviewBench, since grounded precision ignores the extra noise. Limits: closed models, iterative, unknown call count, and the noise hurts the product.
2. **OpenCodeReview is a warning for us.** A precision-tuned design doubled F1 on a precision-sensitive benchmark while recall dropped. ReviewBench grounded F1 runs the other way. This supports including `pool-raw` and `pool-raw-x2`.
3. **Freeze memory before any benchmark.** The ReviewBench image must run with `learn` off and no data branch (rb-01b check).

## Where it agrees
- No Nemotron fine-tuning on Token Factory. Training papers (CoRAL, Desiview KTO, DPO-f+) need 6k–176k pairs and GPUs, and their gains are BLEU or repair, not recall. Closed.
- 3.5 id, price, SWE-bench 71.10. It also warns, as R10 did, that the gap from 60.47 is cross-harness.
- `medium_effort` exists; reasoning shares `max_tokens`. `reasoning_budget` is an NVIDIA client wrapper, not a confirmed Nebius parameter. **This corrects Grok.**
- Skip voting. Rejection memory doesn't create findings, so it doesn't help a benchmark.

## Where the chat disagrees with ChatGPT's Q4
- **#1 dependency context and #2 complementary specialists:** both rest on misattributed or bundled evidence (above). They're new code against a 12-point run spread, with no time to ablate before 29 Oct. **Not before submission.**
- **Its schedule (implement 20–22, ablate 23–25):** this collides with freeze-2 on 24 Oct and the seeded test on 25 Oct. No.
- **Temperature 1.0 check:** still a separate, later variable (R8, R10). It needs a per-stage temperature option.

## P008 amendment?
**Not by the chat. One option for Sunny.** Under P008 every candidate uses existing frozen prompts. A missed-issue pass would be candidate 6 (`pool-raw` + one omissions call, about 1.33× cost), and it needs:
- a new prompt and stage code, frozen before the first test run;
- an rb-01c;
- a split between the reviewer we benchmark and the one the Action ships, unless the Action adopts it too.

`pool-raw-x2` already tests the cheap recall boost. The chat recommends **no**: the evidence is from closed models only, and the split weakens the "same reviewer" claim. If Sunny says yes, it must be written and frozen before the 25-PR test runs.
