# What TraceHound is

**One line (current, since 2026-10-07):** an open pull-request reviewer that runs on NVIDIA Nemotron through Nebius Token Factory. It is measured honestly, with one judge, against five commercial reviewers on a public benchmark, and posts line comments through a GitHub Action.

Repo: [SunnyBagal/TraceHound](https://github.com/SunnyBagal/TraceHound) (public, MIT). Owner: Sunny (SunnyBagal). Hackathon: Nebius x NVIDIA Global AI Hackathon, track Coding and Agentic Engineering. See [hackathon](hackathon.md).

## The two halves of the repo

The repo holds two projects that share code. The first one led to the second.

| | Graph + repair agent (Sept 30 – Oct 7) | PR reviewer (Oct 7 onwards) |
|---|---|---|
| What | Maps a TypeScript repo into an evidence-backed component graph. A repair agent fixes bugs in a sandbox, with or without the graph | Reads a PR and posts line comments on defects |
| Model use | Optional naming pass (Nano); the repair agent (Nano/Super) | Reviewer, verifier and judge (Super) |
| Status | Built, live viewer, evaluated. **The graph did not help the agent** ([D050](../decisions/D050-graph-did-not-help.md)) | In progress. Best dev F 32.8% so far; 059 running |
| Where | `packages/analyzer` (extract, group, aggregate, changes, impact, harness, finder), `viewer/` | `packages/analyzer/src/review/`, `src/bench/`, `packages/action/`, `.github/actions/` |
| History | [phase 1](../history/phase-1-graph.md), [phase 2](../history/phase-2-repair-eval.md), [phase 3](../history/phase-3-finder.md) | [phase 4](../history/phase-4-reviewer.md) |

## Live things

- Viewer (graph canvas and landing page): https://tracehound-tau.vercel.app. Static Next.js on Vercel; it redeploys on every push to main.
- GitHub Action: `SunnyBagal/TraceHound/.github/actions/tracehound-review@main`. Merged (#25, #28), but **not yet run live on any repo**. The demo repo is the next product job.
- Demo repos for the graph: `SunnyBagal/cex-v2-boilercode` @ `da0e3d6` (a fork, used with the author's agreement), and `SunnyBagal/Recall` @ `5d2165a`.

## Product rules that never change

- Nemotron on Nebius is the engine. Other models may only be judges or comparison arms, reported beside it.
- Graph edges are deterministic and backed by evidence (file, symbol, line range, extractor, confidence). No model draws an edge.
- Every model call goes through `src/llm/client.ts`: cache → budget reservation → request → ledger.
- Tests never hit the network.
- Blindness: Cal.com is the reviewer's test set, and no session that builds or tunes the reviewer reads its content ([benchmark](../benchmark/benchmark.md)).
- Every non-obvious choice goes in `docs/decisions.md` with what was rejected ([digest](../decisions/digest.md)).

## The honest headline today

- **Recall:** the diff-only reviewer finds as many real defects as CodeRabbit (43.2% recall on dev).
- **Precision:** about three quarters of its comments are wrong, which is the lowest precision of any reviewer measured.
- **Fixes so far:** every attempt to raise precision has traded away recall, so F hasn't moved past 32.8%.

Full picture: [strengths and gaps](../strengths-and-gaps.md), [results](../results.md).
