# Devpost form answers (paste-ready, 29 Oct)

Every number is from `submission/numbers.md` or the repo docs it cites. Ratings are Sunny's call; the suggested ones come with the reasons judges asked for. A bare "10", "Good" or "N/A" in a field that asks "why" reads as low effort.

---

## Elevator pitch (≤ 200 characters; this one is 173)

Open-source GitHub Action that reviews PRs with NVIDIA Nemotron on Nebius Token Factory. On a pre-registered blind test, statistically tied with 4 of 5 commercial reviewers.

(Your current pitch is accurate too. This one adds the result, which is what makes a judge click.)

## Is your project new or existing prior to August 26, 2026?

**New.** That matches the repo: no code existed before 26 Aug, the history starts 30 Sep, and `docs/devpost/what-changed-since-26-aug.md` explains it. The "significantly updated" field stays N/A.

## Working demo URL (currently empty: fill it)

https://tracehound-tau.vercel.app

## Which model(s) did you use, and why did you choose that size/variant?

We picked each model by measuring it on the job, not by its size.

- **Nemotron 3.5 Super** (`nvidia/Nemotron-3.5-Super-VL`) runs the product: both PR reviewers and the verifier. On 2 runs over our 40-PR dev set it raised mean F from 42.3% (five Nemotron 3 Super runs) to 50.4%, mostly through precision (40.5% → 54.8%, about half the false positives). It costs about twice as much per PR ($0.063 vs $0.031 median), which is still cents.
- **Nemotron 3 Super** (`nvidia/nemotron-3-super-120b-a12b`) writes the suggested fixes, a stage we didn't re-measure on 3.5, and is our benchmark judge.
- **Nemotron 3 Nano** names the components of the repository graph from extracted facts, with reasoning off. That's high volume and low stakes, and Nano tied Super on our naming checks at a fraction of the cost.
- We also tried the 550B **Nemotron 3 Ultra** (free NVIDIA API, report only). Over 2 dev runs it didn't review better than 3.5 Super (F 47.1% vs 50.4%, no interval-backed difference), so for this task bigger wasn't better.

## How would you rate Nemotron's output quality for your use case? (suggested: 8)

8/10.

What worked: with reasoning on, Nemotron 3.5 Super finds real bugs and points at the exact line. In our seeded test it caught 5 of 6 bugs inside the diff and commented on none of the 8 clean pull requests. Against Nemotron 3 Super it cut false positives roughly in half (28–29 vs 40–55 per dev run), and it follows our JSON output format on most chunks.

What fell short:
- On about 7 of 40 dev PRs per run, one chunk came back as invalid JSON even after a retry.
- 79 of 624 calls stopped at the 32,000-token limit, which looks like reasoning using up the budget.
- At temperature 0 the same reviewer still varied from run to run (35.6–48.0% F over five 3 Super runs).
- Cross-file reasoning was the weakest area: 0 of 4 "semantic" cross-file bugs in the seeded test.

## Did you fine-tune, prompt-engineer, or use Nemotron out of the box? What was your approach?

Out of the box: no fine-tuning. The work went into the pipeline around the model and into measuring it.

- **Two views of every change.** One reviewer reads only the diff. The other also gets repository context built without a model: the changed files and `git grep` references to the changed names.
- **Pool, then verify.** A deterministic pool merges the two reviewers' findings and drops overlaps. A third Nemotron pass then reads each finding with the code around it and keeps or drops it.
- **Frozen, pinned prompts.** Prompts are pinned by sha256 in tests, so changing a prompt is a failing test.
- **Every idea was an experiment.** Nine variants were pre-registered and measured on a 40-PR dev set, scored by an LLM judge plus a second judge from another model family. Only then was the winner frozen and run once on a blind test set. Most variants didn't help, and every loss is published.
- **Cross-file contract checks are deterministic**, with no model calls.

## How did Nemotron's performance compare to other models you've used for similar tasks?

We compared models by measuring them rather than going on impressions.

- **Within the Nemotron family** (same 40-PR dev set):
  - 3.5 Super raised mean F over 3 Super from 42.3% to 50.4%: +8.1 points [+1.4, +14.4] under our scorer of record. The second judge's interval included 0.
  - The 550B 3 Ultra didn't review better than 3.5 Super.
  - Nano matched Super on component naming at much lower cost.
- **As a judge:** Nemotron 3 Super agreed with Qwen3-235B, our second judge from another model family, on 78.6% of 1,130 dev comments (kappa 0.809).
- **Against five commercial code reviewers**, using their recorded reviews on the same public benchmark: the Nemotron pipeline was statistically tied with four of five on the blind test and ahead of one. Across all 50 PRs it was behind the leader, tied with the next two and ahead of the last two.
- **Where it didn't transfer:** on ReviewBench test runs 3.5 scored below 3 Super (12.2 vs 13.9 grounded F1), so the dev-set gain didn't carry over everywhere.

## Which Nebius platform capabilities were most valuable to your project and how?

**Token Factory's OpenAI-compatible API is the entire runtime.** There are no GPUs to provision: the GitHub Action calls it live on every pull request with the user's own key, from a standard runner with no GPU. The whole benchmark ran on it too: both reviewers, the verifier, and both judges, including Qwen3-235B as the second judge.

- **Swapping models is a config change.** Moving from Nemotron 3 Super to 3.5 Super, or adding Qwen as a second judge, needed no infrastructure work, which is what made comparing them cheap.
- **Per-call usage feeds our guardrails.** Every call goes through one client: response cache, budget reservation against per-run and total caps, the request, then an append-only spend ledger built from the usage Token Factory returns.
- **Regions mattered.** 3.5 Super is served from us-north1 and 3 Super from us-central1, so the client routes each model to its region per request.
- **Cost:** $57.05 + VAT over the 30 days to 24 Oct for the whole project, and about $0.06–0.07 per review on 3.5.
- **Sandboxes:** we requested Token Factory Sandboxes for the repair agent, but they weren't available on our account (case AISTUDIOSUP-1966), so a hardened local Docker container became the sandbox.

Every Nebius interaction is logged in `FEEDBACK.md`: 29 dated entries with exact error strings and counts.

## How likely are you to recommend running Nemotron on Nebius to other developers? (suggested: 9)

9/10. You get an OpenAI-compatible endpoint, open Nemotron models from Nano to Super under one key, cents per pull-request review, and model switching as a one-line change, so you can measure models against each other instead of guessing. One point off for the rough edges listed under "improvements" below.

## How would you rate your experience running Nemotron inference on Nebius compared to previous cloud or local development environments? (suggested: 9)

9/10. Compared with running models locally on a laptop, there was nothing to host. We ran 120B-class reasoning models at cents per call, straight from a GitHub Actions runner. The weak spot was billing visibility: the console balance trailed billing by hours, and we had to add the 18% VAT to every estimate ourselves.

## What additional features or improvements would have made the Nemotron on Nebius experience more effective for your project?

- **A model list that matches what each region serves.** us-central1's `/v1/models` listed `nvidia/Nemotron-3.5-Super-VL`, but chat completions there answered 404 "does not exist" on 35 of 35 calls; it's served only from us-north1. The region per model should be in the catalog.
- **A reasoning budget the API honours, and documents.** 79 of 624 calls hit our 32,000-token limit, likely reasoning using up the budget; our probe found no budget control honoured.
- **Structured output (JSON schema) confirmed per model**, including whether it works with reasoning on. Invalid JSON cost us a chunk on about 7 of 40 PRs per run.
- **Real-time usage and balance**, VAT included in estimates, and per-key spend caps.
- **Sandboxes on all accounts.** We wanted them for the repair agent but couldn't use them.

## What do you most hope to see from the Nemotron team next?

- **Reasoning-budget control** that serving providers expose, so long reviews finish instead of truncating.
- **Structured output that stays valid with reasoning on.**
- **Guidance for reproducible evaluation runs.** At temperature 0 our results still varied by 12 F points run to run, and a recommended deterministic setting would make model comparisons much cheaper.
- **Stronger cross-file reasoning** on code: bugs that break files the change didn't touch were our clearest gap.
- **Nemotron 3.5 Super in every Token Factory region.**

## Did you use Tavily in your project?

No.

## Builders & Brews IRL city

Leave empty unless you attended one.
