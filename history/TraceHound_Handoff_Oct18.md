# TraceHound handoff, 18 Oct 2026, 22:40 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer on NVIDIA Nemotron 3 Super via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**. Deadline 30 Oct 2026, 22:30 IST; target submit 29 Oct.
- You plan; you don't build. Write every prompt as a **file**, and say which tool, folder and session it goes in.
- **Check every PR from GitHub before Sunny merges.**
  - Clone the public repo and fetch `refs/pull/N/head`.
  - Read the diff and recompute every table.
  - CI: WebFetch the PR's checks page or the Actions run page. `gh api` is blocked in your sandbox; plain git works.
  - When something must not merge yet, put **HOLD #N** in the first line of your reply. Sunny merged #38 early on 18 Oct.
- Keep the **brain vault** current after every event, and push it. To push, attach the repo with `add_repo` (owner SunnyBagal, repo tracehound-brain, access push).
- Style: short, blunt, honest. Sunny wants direct feedback, not reassurance. Every claim is framed honestly.

## The brain (start here)
GitHub `SunnyBagal/tracehound-brain`. Read in order:
1. `NOW.md`: state, the **prompt queue table**, credits, honest claim status.
2. `lessons.md`
3. `ops/runbook.md`: lanes, including the new Claude cloud lane.
4. `results.md`: now includes the **all-50-PR combined table**.

Today's prompts are archived in `prompts/2026-10-18/`.

## Lanes
| Lane | Tool | Folder |
|---|---|---|
| Relay | Claude Code builder + reviewer, started from `~/Projects/tracehound-ops` | `~/Projects/TraceHound` |
| Codex | ChatGPT Codex | `~/Projects/TraceHound-codex` |
| UI | Codex GPT Sol 6.1 | `~/Projects/TraceHound-ui` |
| Claude cloud | claude.ai/code, $96 credit, expires 5 Nov; **3 free Ultrareviews** | repo SunnyBagal/TraceHound |
| Grok | Grok Premium, a fresh chat each time | — |

## State at handoff (verify on GitHub)
- **Main `6058d53`.** Merged 18 Oct, all with merge commits:
  - #35 UI round 1;
  - #36 062 Cal.com result;
  - #32 dogfood;
  - #37 Super summaries;
  - #38 fix suggestions;
  - #39 063 results page and README section.
- **Open: #40**, the codex-06c deleted-line guard (head `d17762d`). Checked by chat: the code is correct and demo #2 is now committable. Merge it once CI is green.
- **Benchmark (bench-judge-v1):**
  - Blind Cal.com run 1: pool-verify-v1 F 58.3, **3rd of 6** (Augment 73.2, Cursor 66.7 | Greptile 50.0, Copilot 46.2, CodeRabbit 36.7).
  - All 50 PRs combined: **4th of 6** (41.2; Greptile 49.5, CodeRabbit 40.0).
  - Dev spread 35.6 → 48.0 across two runs.
- **Claims (Sunny, 18 Oct):** no posts that invite "how did you beat CodeRabbit" questions. Wording: "3rd of 6 on 10 blind PRs, 4th of 6 across all 50, losses published". The goal is still **top 2**, judged on all 50 PRs.
- **Nebius:** console $39.93; ledger $17.76; total cap $45. GitHub Action runs (dogfood and demo) spend off the ledger, about $1 so far.

## In flight (prompt queue). Sunny is pasting the outputs back one at a time
| File | Lane | What to check when the output arrives |
|---|---|---|
| `064-vote.md` | Relay, cap $10, dev only, draft PR | Freeze commit before the first model call; runs 4–6 launched detached; vote tables recompute; adoption rule (≥ +5 F over the 5-run baseline mean, P up, holds under x1) applied as written; no Cal.com; then **Ultrareview #2** on the branch |
| `codex-07-data-branch.md` | Codex, after #40 merges | Dogfood re-pinned to #40's merge sha; `data-branch` opt-in; never fails the review job; live check on the demo repo; then **Ultrareview #1** |
| `ui-02-reviews-dashboard.md` | UI | No HTML injection of comment text; rate-limit state; bug cards + fix diff + Commit link; benchmark page numbers copied, not invented; $0 Nebius |
| `cc-01-independent-audit.md` | Claude cloud | Its mismatches and unsupported claims, and whether they're real; fold its fixes into the judges' README job |
| `cc-02-reviewbench-spike.md` | Claude cloud | Feasible? Licence, judge cost, adapter skeleton. If feasible: a 60-PR subset run is the strongest answer to "10 PRs is too small" |
| `grok-05-precision-research.md` | Grok | Kill test: open every link. Precision techniques → candidates for 065 if voting loses |
| `grok-06-nebius-nvidia-levers.md` | Grok | Kill test. Hackathon criteria → write-up; batch or caching pricing → cost |

## Next, in order
1. Process the outputs above. Update NOW, prs, results and lessons after each.
2. If 064 adopts a variant: 065 runs it once on Cal.com, pre-registered (state it as a second look at the test set), and reports the all-50 table.
3. ui-03: graph overlay from `tracehound-data` (after codex-07). Then the judges' README (fold in cc-01), Grok audit, Devpost write-up, video under 3 min, **submit 29 Oct**.
4. X post: was planned for Mon 19 Oct. Ask Sunny whether it's still on, and check Grok's grok-04 draft against `results.md` and the agreed wording.

## Side project
**TariffLens** (Sunny's AI portfolio project, replacing FactLens) starts 30 Oct. The chat reviewed it on 18 Oct: good pick; fix schedule-revision label validity, redaction leakage, near-duplicate dedup, an encoder baseline, and scope.
