## Inspiration

I started TraceHound because coding agents rebuild their understanding of a repository on every task, while important relationships, like an API talking to a matching engine through Redis, stay invisible to import graphs. It began as an evidence-backed repository map with a repair agent on top. Then a pre-registered evaluation (8 seeded bugs, 160 runs) showed the map didn't help the agent: with the graph on vs off it resolved 11 vs 13 of 40 bugs on Nemotron Nano and 28 vs 26 of 40 on Nemotron Super, and the agent never called a graph tool in 80 graph-on runs. So I published that and turned the project into a pull-request reviewer. The principle stayed the same: a claim about code should be checkable, and results get published, losses included.

## What it does

TraceHound is an open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on Nebius Token Factory. Add one workflow file and a Nebius API key, and every same-repository pull request gets one review, directly in GitHub.

- **Line comments:** findings on the exact diff lines, labelled model-generated and unverified.
- **Suggested fixes:** patches in GitHub's native suggestion blocks, with a *Commit suggestion* button, when a proposal passes our structural checks.
- **Cross-file contracts:** a deterministic check that lists files the PR did *not* edit but which depend on an export, route, queue, Redis key, environment variable or event it changed. No model calls.
- **A dashboard:** reviews as bug cards with their fixes, the benchmark with its intervals, and the repository graph, where a pull request lights up the components it touches and every edge links to its file, symbol and line range.
- **Maintainer feedback (optional, off by default):** suppresses findings similar to comments maintainers rejected, and records each suppression. Not measured by the benchmark.

Judges can browse the recorded reviews, the dashboard and 30 seeded PRs without a key. Triggering a new review needs the Action installed in a repository you control, with your own key; fork PRs are skipped because forks get no repository secrets.

## How we built it

The review pipeline looks at the same change in different ways:

1. A **diff reviewer** reads the PR title, description and patch.
2. A **context reviewer** also reads the changed files and the places that reference the changed names.
3. A **deterministic pool** merges their findings and drops overlapping reports.
4. A **context verifier** reads each pooled finding with the code around it and keeps or drops it.

A separate fix stage proposes small patches for kept findings. A proposal must pass size, placement and changed-line checks before it appears as a suggestion. Those checks don't prove the patch passes the repository's tests.

### How Nebius and NVIDIA are used

| Task | Model or service |
|---|---|
| Diff review, context review, verification | Nemotron 3.5 Super via Nebius Token Factory |
| Suggested fixes | Nemotron 3 Super via Nebius Token Factory |
| Naming graph components from extracted facts | Nemotron 3 Nano via Nebius Token Factory |
| Cross-file contract checks | Deterministic analysis, no model calls |

**Why 3.5 Super:** on 2 dev runs it raised mean F from 42.3% (five 3 Super runs) to 50.4%, mostly through precision (40.5% → 54.8%). The paired bootstrap difference was +8.1 points [+1.4, +14.4] under our scorer of record; under the second judge the interval included 0. It costs about twice as much per PR.

The stack is TypeScript on Node.js, pnpm workspaces, ts-morph, Zod and a composite GitHub Action. The dashboard is Next.js, React, React Flow and ELK on Vercel. The earlier repair harness used Docker, because Token Factory Sandboxes weren't available on our account at the time.

Every model call goes through one client with a response cache, budget reservations, spending caps and an append-only ledger. If a reviewer chunk fails, the Action keeps the successful findings and labels the review partial.

Evaluation was part of the build: pre-registered experiments, frozen configurations, prompts pinned by hash, a second judge from another model family, and every decision logged with the alternative we rejected. The measured review pipeline is frozen at `review-freeze-2`; the Action at `review-freeze-2.1` fixes installation only.

The code and documentation were written with AI coding assistants, including Claude Code, Codex, Devin, Grok and Gemini.

## Challenges we ran into

**The original idea failed its own test.** The graph was meant to help a repair agent, and the evaluation showed it didn't. We published the result and moved to PR review, keeping the graph as a viewer instead of claiming it powers the reviewer.

**Runs varied even at temperature 0.** Five runs of the same frozen 3 Super reviewer ranged from 35.6% to 48.0% F on our 40-PR dev set. That's bigger than most effects we tried to measure, so we moved to repeated runs, means and uncertainty intervals.

**Serving details changed the product.** Nemotron 3.5 had to be routed to Token Factory's us-north1 endpoint, and it returned invalid JSON for a chunk on about 7 of 40 PRs per dev run. We made partial reviews visible instead of presenting incomplete coverage as a full review.

**The end-to-end test found a bug in our own Action.** Its install step failed on repositories that pin another pnpm version. We fixed the install path without touching the measured review code.

**A dev-set win didn't transfer automatically.** In ReviewBench test runs (25 PRs, one round, ReviewBench's own judge), the shipped 3.5 pipeline scored 12.2 grounded F1, against 13.9 for the same pipeline on 3 Super. These were test runs, not a leaderboard submission; we didn't fund a final run.

## Accomplishments that we're proud of

**A reviewer people can use and inspect.** Findings and suggested changes live in GitHub; the dashboard makes the reviews and measurements easy to explore. On our 40-PR dev set, across 2 dev runs, the 3.5 pipeline's median cost was $0.063 per PR before VAT, excluding suggested-fix generation.

**A pre-registered blind test of the shipped configuration.** On 10 blind PRs with 23 reference comments:

| Metric | Result |
|---|---|
| Precision | 63.2% (12/19 findings matched) |
| Recall | 52.2% (12/23 reference comments matched) |
| F score | 57.1% |

On this small test, TraceHound is statistically tied with four of five commercial reviewers and ahead of one. The configuration was chosen on dev before this run, with no tuning after it. This was the second blind look, after an earlier run of the 3 Super configuration.

Across all 50 benchmark PRs (n = 97), F was 52.2%: behind the leading commercial reviewer, statistically tied with the next two and ahead of the last two (3rd of 6 as a point estimate). 40 of those 50 PRs are our dev set, which we tuned on.

**A seeded test on the live workflow, with clean controls.** On 30 pre-registered seeded PRs, TraceHound caught 9 of 22 seeded defects and stayed silent on all 8 clean controls. It caught 5 of 6 bugs inside the diff but only 4 of 16 that break files the PR didn't touch, and 3 of those 4 came from the deterministic contract check, not the model.

| Seeded defect category | Caught |
|---|---|
| Inside the diff | 5/6 |
| Cross-file, typed | 2/6 |
| Cross-file, keyed | 2/6 |
| Cross-file, semantic | 0/4 |
| All seeded defects | 9/22 |
| Clean controls receiving a comment | 0/8 |

These ran on mirrors of my own applications, scored automatically by a rule committed before any PR opened. No other reviewer was run on them. The report discloses every deviation: the install fix between mirrors, script changes, workflow re-triggers, and third-party comments excluded from scoring.

**Every loss is published.** The graph, extra verifier designs, voting across runs, a recall-first variant and a larger model all failed to show the gains we hoped for. The repository records each one next to the result we kept.

## What we learned

- **An architecture is a hypothesis until it's tested.** Building a repository graph didn't mean an agent would use it or benefit from it.
- **More context and more stages are hypotheses too.** Their value depends on what reaches the model, what gets filtered out, and which failures the metric counts.
- **Precision and coverage pull against each other.** A verifier removes noise and also drops real findings. We're still working out when that trade pays.
- **Deterministic analysis deserves its own credit.** Most cross-file catches in the seeded test came from contract checks; crediting them to Nemotron would misrepresent the result.
- **The benchmark and the product must fail the same way.** Model choice, chunk failures and comment caps change what users actually get.
- **Honest limits make results useful.** A small blind test, a tuned dev set and seeded changes answer different questions, so we report them separately.

## What's next for TraceHound

- **Recover reviewer chunks lost to invalid JSON.** The reviewer's retry doesn't tell a reply cut off at the token limit from malformed JSON (an automated code review found this). Fixes are on main behind off-by-default options and will be measured before they change the frozen configuration.
- **Improve cross-file coverage.** The seeded test caught no semantic cross-file defects. That's the clearest gap.
- **Re-evaluate the verifier** by comparing the findings it removes with the noise it saves, on more than one dataset.
- **Test noise controls on saved runs first.** Comment caps and cheaper voting are still hypotheses; replaying saved records lets us check them before paying for new calls.
- **Keep the evidence public.** Every future change ships with its configuration, cost, uncertainty and losses.

## Try it

- Source code and install: https://github.com/SunnyBagal/TraceHound
- Dashboard, reviews, graph and benchmark: https://tracehound-tau.vercel.app
- Demo repository: https://github.com/SunnyBagal/tracehound-demo
- Results, intervals and every loss: https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/results.md
- Seeded test, pre-registered kit and scores: https://github.com/SunnyBagal/tracehound-seeds/blob/main/RESULTS.md
- Every decision with the alternative we rejected: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md
- Video: YOUR_YOUTUBE_URL
