# TraceHound video script v2 (Sunny's flow: home → reviewer → benchmark → graph), ≤ 2:50

Rules (Devpost: under 3 minutes, public on YouTube, shows it working, **no third-party trademarks without permission**; numbers from `submission/numbers.md` only):
- **Blur the hero's agent logos and the reviewer names/logos on /benchmark in the edit.** A disclaimer isn't permission, so it doesn't satisfy the trademark rule. NVIDIA and Nebius may show where our model id or their own UI shows them.
- Say no other reviewer's name out loud.
- Every review on screen is the current config (header says `nvidia/Nemotron-3.5-Super-VL`). Never show the old Nano reviews on the demo repo.

Before recording: open these in tabs so there's no loading on camera.
- `https://tracehound-tau.vercel.app`
- `https://github.com/SunnyBagal/recall-seeded/pull/4` (model catch: check-then-act race)
- `https://github.com/SunnyBagal/recall-seeded/pull/7` (contract-check catch: renamed route used in an unedited file)
- `https://tracehound-tau.vercel.app/benchmark`
- `https://tracehound-tau.vercel.app/graph` with the Recall snapshot, PR lane showing recall-seeded

| # | time | on screen | voice-over |
|---|---|---|---|
| 0 | 0:00–0:08 | Black card, white text: "Third-party logos are shown only to name the coding agents whose pull requests TraceHound reviews. They are trademarks of their owners; no affiliation or endorsement. TraceHound's reviewer runs only on NVIDIA Nemotron via Nebius Token Factory." (Logos in the video are blurred.) | (no voice, or:) "A quick note: the reviewer runs only on NVIDIA Nemotron, through Nebius Token Factory." |
| 1 | 0:08–0:30 | Homepage, scroll slowly top to bottom. Hero's agent marks blurred. Pause on "Built to be checked." | "This is TraceHound: an open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on Nebius Token Factory. One workflow file and one API key, and every pull request gets a review, right inside GitHub." |
| 2 | 0:30–1:00 | GitHub, recall-seeded PR #4: the review header with `nvidia/Nemotron-3.5-Super-VL`, then the line comment on the race. If a suggestion block is there, hover *Commit suggestion*. | "Here's a real review. Two Nemotron 3.5 Super reviewers read the change, one sees only the diff, the other also reads the code around it. Their findings are pooled, and a third Nemotron pass verifies each one before it's posted. This one caught a check-then-act race, on the exact line." |
| 3 | 1:00–1:15 | recall-seeded PR #7: the "Files you didn't edit that depend on this change" section. | "And this part has no model at all. A deterministic contract check lists files the pull request didn't touch that still use what it changed. Here, a renamed route that another file still calls." |
| 4 | 1:15–1:50 | /benchmark: the blind-test section first (names and logos blurred; TraceHound row visible), then scroll to "Dev and blind sets together", then the seeded numbers if shown, ending on "Limits". | "We measured it before we said anything about it. On a pre-registered blind test of ten pull requests, F was fifty-seven percent: statistically tied with four of five commercial reviewers, and ahead of one, on only ten PRs. Across all fifty, it's behind the leading reviewer, tied with the next two, ahead of the last two. On thirty seeded pull requests it caught nine of twenty-two bugs and stayed silent on all eight clean ones. Every loss is published on this page too." |
| 5 | 1:50–2:35 | /graph, Recall: the component map. Pick a PR in the PR lane → changed components light up, the rest dim → open a component → the right sidebar: its files, then one edge's evidence (file, symbol, line range). Show "outside the graph" if present. | "This is where the project started: a map of the repository, where every edge points back to a file, symbol and line range. Pick a pull request and you see which parts of the system it touches. Open a component, and the sidebar shows its files and the evidence behind every connection, so a review comment lands on the architecture, not just on a line." |
| 6 | 2:35–2:50 | README "Try it" YAML, full screen, then end card: `github.com/SunnyBagal/TraceHound` · `tracehound-tau.vercel.app` · MIT. | "Add your Token Factory key as a secret, save this one file, and it's running. It's MIT licensed. TraceHound." |

Timing check: 8 + 22 + 30 + 15 + 35 + 45 + 15 = 170 s = 2:50. Shot 4's voice-over is the longest; read it at a steady pace and cut "Every loss is published on this page too" if you run over.
