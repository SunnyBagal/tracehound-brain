# R7 · GitHub ReviewBench feasibility (cc-02, Claude cloud)

**Source:** Claude cloud session, 18 Oct; draft PR **#42** (`scripts/reviewbench/`, `docs/bench/reviewbench-spike.md`).
**Chat check (18 Oct):** the session couldn't read the ReviewBench repo, but **the chat's sandbox can clone `github.com/review-bench/ReviewBench`**. It has `AGENT_CONTRACT.md`, `corpus/`, `agents/` and `examples/`, and the licence is MIT. So blocker 1 is solvable: the next cloud or relay job gets the contract text pasted in, or reads it from a session that has the repo.

**Chat's reading:**
- **This is the best answer to "10 PRs is too small".** It's a third-party benchmark in which GitHub itself ran Copilot, Devin, Qodo, Cubic and Greptile. Published grounded F1: Copilot 40.1, Devin 37.0, Qodo 35.1, Cubic 27.3, Greptile 27.2.
- **Costs:** about $3.5–4 for the 25-PR practice set; about $9 per stratified-60 run, mostly the judge.
- **The judge needs Anthropic API money,** which is not the Nebius credit, and probably not the Claude cloud credit either. Ask Sunny.
- TS + JS is only 38% of the set; our context reviewer is weakest on C# and less common languages.
- Pre-register before running, as in 061.

---

Status: the blog host (github.blog) and review-bench.ai are blocked by this session's network policy, so I'm going through secondary write-ups and trying to attach the benchmark repo through GitHub.

I can't read the ReviewBench repo directly: attaching it was denied, and so was a plain clone. I'll get what I can from web search and mark anything unverified, then build the adapter.

Research is mostly done (the exact field names stay unverified). Now reading our reviewer entry points.

Writing the adapter skeleton now.

Now fixtures, test config and tests.

Adapter tests pass (11) and the adapter typechecks. Running the repo gates before the first commit.

Now the spike doc. Checking an existing bench doc's style first.

Pushed. Opening the draft PR (checking for a template first).

I opened the draft PR [SunnyBagal/TraceHound#42](https://github.com/SunnyBagal/TraceHound/pull/42) and did not merge it. One caveat comes first: this session could not read ReviewBench itself. Its GitHub access only covers this repo, and attaching or cloning `review-bench/ReviewBench` was refused. The blog post and leaderboard sites were blocked too. So every fact below comes from web-search summaries. The doc labels each one by source, and the guesses about their file formats are all in one file, `scripts/reviewbench/format.ts`.

**Feasible: probably yes, not confirmed.** ReviewBench gives each PR as a base and head commit in a mirror repo, and expects findings back as `{file, start_line, end_line, message}`. Our pipeline needs a diff plus a checkout at the head commit, and produces path, line and body, so the two fit without changing the frozen reviewer.

The adapter in `scripts/reviewbench/` has three steps, none of which calls a model:
- **`prepare`:** builds our reviewer's input from the diff between a PR's base and head commits.
- **`submit`:** turns the verifier's kept findings into ReviewBench findings.
- **`subset`:** picks a language-stratified subset, fixed by a seed.

Its 11 tests pass on a hand-made 2-PR fixture and it typechecks. `pnpm test` and `pnpm typecheck` were green before each commit, but they don't cover this folder: it isn't a workspace package, and wiring it in would mean editing files outside the paths you allowed.

**What I found on your questions:**
- **URLs and licence:** the dataset, rubric, judge and runner are all in `github.com/review-bench/ReviewBench`, and the README says MIT. The dataset has no separate licence that I could find, and the PR code belongs to 187 repos under their own licences, so I committed none of their data.
- **Data format:** each task is a repo, PR number, base and head commit, with one golden-findings file per task. There is a 25-PR practice set and the full 219.
- **Plugging in:** you supply a container image, which their runner starts once per PR; it must write a findings file. The mount paths and variable names are still unknown.
- **Judge:** Claude Sonnet 5, called with `npm run judge`; you pick the provider and pay. The number of calls per PR is unknown, so the costs below use three scenarios.
- **Published results (benchlm.ai mirror, 12 Oct):** grounded F1 for Copilot 40.1%, Devin 37.0%, Qodo 35.1%, Cubic 27.3%, Greptile 27.2%. GitHub ran all of these itself.
- **Language mix:** TypeScript 68, Python 41, C# 25, Go 19, JavaScript 15, other 51. TypeScript plus JavaScript is only 38% of the set. Our context reviewer has no name-matching patterns for C# and the less common languages, so it is weakest on about a third of the PRs.

**Blockers, in order:**
1. **Repo access:** someone needs to read the ReviewBench repo at a fixed commit and correct `format.ts`. That needs your permission: approve attaching `review-bench/ReviewBench` to a session, or allow `add_repo`.
2. **Network:** it's unknown whether their container lets our reviewer reach Nebius. If it doesn't, our reviewer can't run inside it.
3. **Judge billing:** the judge runs on an Anthropic account and costs real money, not Nebius credit. ReviewBench reportedly pays for the judge only on a final leaderboard submission.
4. **Spend cap:** about $27 is left under the $45 Nebius cap (ledger at $17.76 after decision 062).
5. **Pre-registration:** our reviewer is frozen, so a ReviewBench run needs a plan written first, like decision 061.
6. **Run time:** the full set takes about 8 hours per run, so it has to run in the background and be able to resume.

**Cost table.** Our side is $0.03–0.05 per PR. The judge is priced at $2/$10 per million input/output tokens, with three scenarios: A (one matching call, about $0.023 per PR), B (an extra call for each finding the golden set doesn't match, about $0.11) and C (one call per finding with full context, about $0.50).

| | 25 PRs | 60 PRs | 219 PRs |
|---|---|---|---|
| Ours | $0.75–1.25 | $1.80–3.00 | $6.57–10.95 |
| Ours with 5× voting | $3.75–6.25 | $9.00–15.00 | $32.85–54.75 |
| Judge A / B / C | $0.58 / $2.75 / $12.50 | $1.38 / $6.60 / $30.00 | $5.04 / $24.09 / $109.50 |
| Total, single run, judge B | ≈ $3.5–4.0 | $8.40–9.60 | $30.66–35.04 |

At $3/$15 judge pricing, multiply the judge rows by 1.5. The official protocol is three runs of all 219, which costs us $20–33 on our side alone; at the top of that range it doesn't fit under the remaining cap.

**Recommended subset:**
1. **The 25-PR practice set first.** It is cheap, tests the whole contract, and measures the judge's real token use per PR.
2. **Then a stratified 60 from the 219, run twice:** TypeScript 19, other 14, Python 11, C# 7, Go 5, JavaScript 4. That is six times our current 10 blind PRs. With scenario-B judging, one run costs about $8.40–9.60, mostly the judge.
3. **The full 219 three times only for a leaderboard entry.**

Sources: [GitHub blog](https://github.blog/ai-and-ml/github-copilot/reviewbench-an-open-benchmark-for-ai-code-review/), [ReviewBench repo](https://github.com/review-bench/ReviewBench) and its PRs [#56](https://github.com/review-bench/ReviewBench/pull/56), [#57](https://github.com/review-bench/ReviewBench/pull/57) and [#58](https://github.com/review-bench/ReviewBench/pull/58), [benchlm mirror](https://benchlm.ai/benchmarks/reviewbench), [JPHutchins/code-review#282](https://github.com/JPHutchins/code-review/issues/282), [beiyanpiki/pir#76](https://github.com/beiyanpiki/pir/pull/76), [The New Stack](https://thenewstack.io/github-reviewbench-code-review/), [DevX](https://www.devx.com/coding/what-is-reviewbench-ai-code-review-benchmark/), [Sonnet 5 pricing](https://anotherwrapper.com/tools/llm-pricing/claude-sonnet-5) and [Verdent's conflicting price](https://www.verdent.ai/guides/claude-sonnet-5-pricing-reality-check).

Do you want me to watch the PR and handle review comments or CI failures as they come in?