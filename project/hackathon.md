# Hackathon: rules and submission checklist

Nebius x NVIDIA Global AI Hackathon, on Devpost: https://nebiusglobalaihackathon.devpost.com. Rules last checked 2026-10-15.

- **Deadline:** 30 Oct 2026, 10:00 PDT, which is 22:30 IST. **Our target is to submit on 29 Oct.**
- **Track:** Coding and Agentic Engineering.
- **Hard rule:** the product must call Nebius Token Factory (or run on Nebius AI Cloud) and use at least one NVIDIA open model. For us, Nemotron is the engine; other models may only be judges or comparison arms.
- **Judging:** a pass/fail viability stage, then four equally weighted criteria: technological implementation, design, potential impact, quality of the idea. Leaderboard rank on a benchmark is **not** a criterion.
- **Feedback prizes:** ten $100 prizes for the most valuable feedback. Our source is `FEEDBACK.md`, which has 17 dated entries so far.
- No official hashtag. When posting, tag @nebiusai and @devpost. The NVIDIA handle is not verified.

## Required deliverables

| Requirement | Status (2026-10-16) | Owner / when |
|---|---|---|
| Working demo URL, free for judges until 15 Dec | Viewer is live (tracehound-tau.vercel.app), but it shows the graph, not the reviewer. **The reviewer demo (a repo with the Action posting real comments) doesn't exist yet** | Codex, 17–18 Oct |
| Public YouTube video under 3 minutes | Not started | 27–29 Oct; Grok drafts the script |
| Public repo, open licence | Done (MIT) | — |
| README that highlights Nemotron and Token Factory | The README still describes the graph and repair agent; it needs a reviewer-first rewrite | Codex, 21–22 Oct |
| Feedback on Nebius/NVIDIA tools | `FEEDBACK.md`, 17 entries, appended by every session that calls Nebius | Cleanup 26 Oct |
| Summary of what changed since 26 Aug (the repo predates the hackathon) | Not written. **Open question:** TraceHound's git history starts 30 Sep, and decisions 001–031 first appear that day, likely carried over. Is the earlier history (FixFlow) elsewhere? | Sunny to answer; write 26 Oct |
| Devpost write-up | Not started | 26 Oct |

Devpost "Built with" list (draft): nvidia-nemotron, nebius-token-factory, typescript, node.js, ts-morph, react, next.js, vercel, docker, vitest, github-actions.

## What judges will probably weigh, and where we stand

| Criterion | Our strongest evidence | Our weakest point |
|---|---|---|
| Viability (pass/fail) | Action merged; CI green; live viewer | No live reviewer demo yet. **This is the top risk** |
| Technical implementation | Pre-registered experiments, freeze commits, one-judge benchmark of five commercial tools, Docker sandbox, deterministic evidence graph | Reviewer quality: F 32–33% vs 41–64% for the commercial tools |
| Design | Viewer UI (Railway-style canvas, landing page) | Review comments' look in a real PR is unproven |
| Impact | Open, cheap (about 1–5 cents per PR on our estimated ledger), self-hostable reviewer on an open model | Precision too low for daily use as is |
| Idea quality | Honest measurement as the product story; "we report our losses" | Reviewers are a crowded market |

Pitch direction: see [P004](../decisions/P004-target-reset.md) (proposed, not yet agreed).
