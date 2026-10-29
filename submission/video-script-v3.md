# TraceHound video script v3 (≤ 2:50): home → reviewer → benchmark → graph → install

Since PR #69: third-party logos are blurred on the site, and the benchmark page shows the five commercial reviewers as **Reviewer A–E**. So the whole site can be filmed as it is. Still: **never say a competitor's name out loud.** Every number below is from `submission/numbers.md`.

**Before recording**
- #69 merged and Vercel redeployed (the benchmark page says "Reviewer A–E" and the logos look blurred).
- Open in tabs, scrolled to the top:
  1. `https://tracehound-tau.vercel.app`
  2. `https://github.com/SunnyBagal/recall-seeded/pull/4` (model catch: check-then-act race)
  3. `https://github.com/SunnyBagal/recall-seeded/pull/7` (contract check: renamed route still used in an unedited file)
  4. `https://tracehound-tau.vercel.app/benchmark`
  5. `https://tracehound-tau.vercel.app/graph`, Recall snapshot, with the PR lane visible
- Browser at 100% zoom, bookmarks bar hidden, notifications off.

| # | time | on screen | voice-over |
|---|---|---|---|
| 1 | 0:00–0:06 | Black card: "Third-party logos are blurred and reviewer names replaced. TraceHound's reviewer runs only on NVIDIA Nemotron via Nebius Token Factory." | (silent, or) "TraceHound runs only on NVIDIA Nemotron, through Nebius Token Factory." |
| 2 | 0:06–0:28 | Homepage: start on the hero, scroll slowly to the bottom, pause on "Built to be checked." | "This is TraceHound, an open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on Nebius Token Factory. One workflow file and one API key, and every pull request gets a review, right inside GitHub." |
| 3 | 0:28–0:55 | recall-seeded PR #4: zoom on the review header showing `nvidia/Nemotron-3.5-Super-VL`, then the line comment on the race. | "Here's a real review. Two Nemotron 3.5 Super reviewers read the change: one sees only the diff, the other also reads the code around it. Their findings are pooled, and a third Nemotron pass checks each one before it's posted. This one caught a check-then-act race, on the exact line." |
| 4 | 0:55–1:08 | recall-seeded PR #7: the "Files you didn't edit that depend on this change" section. | "This part uses no model at all. A deterministic check lists files the pull request didn't touch that still use what it changed. Here, a renamed route another file still calls." |
| 5 | 1:08–1:45 | /benchmark: the headline tiles, the blind-test figure, scroll through "Dev and blind sets together", then the seeded numbers if shown, end on Cost and Limits with the wolves. | "We measured it before claiming anything. On a pre-registered blind test of ten pull requests, F was fifty-seven percent: statistically tied with four of five commercial reviewers, and ahead of one. Across all fifty, it's behind the leader, tied with the next two and ahead of the last two. On thirty seeded pull requests it caught nine of twenty-two bugs and stayed silent on all eight clean ones. And every loss is on this page, with its limits." |
| 6 | 1:45–2:33 | /graph, Recall: the component map. Click a PR in the PR lane → changed components light up, the rest dim → click a component → the right sidebar: its files, then one connection's evidence (file, symbol, line range). | "This is where TraceHound started: a map of the repository, where every connection points to a file, a symbol and a line range. Pick a pull request and you see which parts of the system it touches. Open a component, and the sidebar shows its files and the evidence behind each connection. So a review lands on the architecture, not just on a line." |
| 7 | 2:33–2:50 | README "Try it" YAML, full screen. Then end card: `github.com/SunnyBagal/TraceHound` · `tracehound-tau.vercel.app` · MIT. | "Add your Token Factory key as a secret, save this one file, and it's running. MIT licensed. TraceHound." |

Total: 6 + 22 + 27 + 13 + 37 + 48 + 17 = 170 s = 2:50. If you run long, cut "And every loss is on this page, with its limits" from shot 5 and shorten shot 6's last sentence.

**Upload:** YouTube, visibility **Public** (Devpost requires public, not unlisted). Title: "TraceHound: PR review on NVIDIA Nemotron via Nebius Token Factory". Then send the URL to the chat.
