# R8 · Which competitors we can run, how they learn, cross-file claims (grok-07)

**Source:** Grok, 19 Oct 2026, prompt `prompts/2026-10-19/grok-07-competitors-learning.md`. The raw output is in `grok-07-competitors-learning.out.md`.

**Check (chat, 19 Oct 12:40): the links didn't survive.** Sunny's paste has none, because Grok's citation chips were stripped. That leaves the double spaces before each `|`. The P003 kill test (opening 5 cited URLs) couldn't be run, so the chat checked the claims that change decisions against primary pages instead.

| Claim | Check | Result |
|---|---|---|
| CodeRabbit's terms ban publishing benchmark results | [coderabbit.ai/terms-of-service](https://www.coderabbit.ai/terms-of-service) §4.2(iv): "disclose the results of any benchmarking of the Services" | **True.** Grok's date (2 Feb 2026) is wrong; the page says last updated 20 Apr 2026 |
| CodeRabbit OSS plan: free, public repos, 1–10 reviews/hour, no card | [docs.coderabbit.ai/management/plans](https://docs.coderabbit.ai/management/plans) | True. Also: **repos with < 10 stars must trigger reviews manually** |
| Greptile Starter: free, 1 developer, 50 credits/month, base review = 1 credit; OSS program; 14-day trial | [greptile.com/pricing](https://www.greptile.com/pricing) | True |
| Bugbot: `.cursor/BUGBOT.md`, runs automatically, 14-day trial | [docs.cursor.com/bugbot](https://docs.cursor.com/bugbot) | True. Pricing **conflicts**: the docs say $40/month for up to 200 PRs after the trial; Grok says usage-based at about $1–1.5 per run. **Only runs on PRs you author** |
| Nemotron 3 Super: 1M context; NVIDIA recommends temperature 1.0, top_p 1.0 (tool calling 0.6 / 0.95) | [unsloth.ai Nemotron 3 Super guide](https://unsloth.ai/docs/models/nemotron-3/nemotron-3-super), quoting NVIDIA | True (secondary source quoting NVIDIA) |
| Nebius has embeddings, e.g. Qwen3-Embedding-8B at $0.01 per 1M input tokens | [getmaxim Nebius listing](https://www.getmaxim.ai/bifrost/model-library/compare/nebius/qwen3-embedding-8b.md) | Price true (third-party listing). Dimensions not confirmed |

Unchecked: Copilot, Augment, Qodo and Graphite rows; the Q2 mechanisms; the Q3 paper list. **Don't quote any of them publicly** until a link is opened.

## What changes because of this

1. **CodeRabbit is out of any test we run ourselves.** Running it on our seeded PRs and publishing the result would breach its terms. Our existing CodeRabbit numbers come from the public benchmark's **recorded** reviews, not from runs we made. The results doc must say exactly that. (This isn't legal advice; Sunny decides.)
2. **Seeded test competitors (cc-04):** Greptile Starter (free; 30 PRs ≈ 30 credits) and Bugbot inside its 14-day trial. **Sunny must open the PRs himself**, since Bugbot only reviews PRs its user authored. Before publishing, Sunny reads both tools' terms for a benchmark clause. Copilot only if Sunny already has a plan. Qodo needs 200 stars, and Augment's OSS route is an application, so both are out.
3. **S8 conventions input (codex-09):** read `AGENTS.md`, `CLAUDE.md`, `.cursor/BUGBOT.md`, `.github/copilot-instructions.md` and `.github/instructions/*.instructions.md` (with `applyTo`), plus our own `.tracehound/rules.md`. That gives zero-setup adoption on repos already configured for other tools.
4. **S8 feedback:** 👍/👎 reactions, threads resolved without a commit touching the line, and replies. This matches Greptile's published approach, which was the only one with a number (R4: address rate 19% → 55%). Nebius embeddings at about $0 make a semantic filter cheap. **Lexical first** (deterministic and testable); embeddings stay a flagged option.
5. **Cross-file isn't ours alone.** Greptile ("second order effects of the diff") and Augment both claim it. So we never say "only TraceHound catches cross-file breaks". The claim is the measured result on the pre-registered seeded test, against the tools we ran.
6. **Temperature:** we run Super at 0, while NVIDIA recommends 1.0. Our runs at temperature 0 already spread 12 F points (064). Changing it would be a new variable. **Not before freeze-2**; note it as a limit.
