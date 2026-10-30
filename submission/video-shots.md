# TraceHound video: shot list (2:50)

What's on screen, shot by shot. The words are in `video-voiceover.md` (same shot numbers and times). Supersedes `video-script-v3.md`; same order: opening card → homepage → recall-seeded PR #4 → PR #7 → /benchmark → seeded-test card → /graph with PR #4 → install.

v2 (30 Oct): shot 2 scrolls the **whole homepage** and shot 5 the **whole benchmark page**. The voice doesn't speed up: the scroll shows more than the words say, and it **stops only where the voice points**. Shot 6 shows the seeded-test card (`tracehound-09-seeded-test.jpg`) instead of RESULTS.md, because the seeded results aren't on the site.

Arc: the problem (1) → what TraceHound does (2) → a real catch (3, 4) → how we know (5, 6) → what didn't work (7) → install in one file (8).

## Before recording

**Site state**
- [ ] https://tracehound-tau.vercel.app/benchmark shows **Reviewer A–E** and blurred logos (`BLUR_THIRD_PARTY_MARKS = true`). On the homepage hero, every mark except NVIDIA's is blurred.
- [ ] The benchmark page **alternates dark and light sections by design** (light: "Dev and blind sets together", "Other runs"). That's correct, don't try to fix it.
- [ ] `/graph` opens on **SunnyBagal/Recall** and `/reviews` on **SunnyBagal/recall-seeded** (default since `2b960a6`).

**Browser**
- [ ] One Chrome window at **1920×1080**, browser zoom **100%** on every tab (⌘0). Zoom in the editor or with the recorder's auto-zoom, never with browser zoom (it reflows the page).
- [ ] Bookmarks bar hidden (⌘⇧B), extension icons unpinned, no other windows visible.
- [ ] Never hover over a tab: Chrome's hover card ("Memory usage …") has ended up in a screenshot before.
- [ ] GitHub in dark theme.
- [ ] **Practise the two long scrolls once** (shots 2 and 5) with a timer. Scroll with the trackpad, steady, not jumping.

**Mac**
- [ ] Do Not Disturb on, Slack/Discord/Mail closed, Dock hidden.
- [ ] Mic check: one 10-second test take, listen back.

**Tabs, in this order, each scrolled to the top**
1. Opening card (a full-screen slide or image; text in shot 1)
2. https://tracehound-tau.vercel.app
3. https://github.com/SunnyBagal/recall-seeded/pull/4
4. https://github.com/SunnyBagal/recall-seeded/pull/7#pullrequestreview-5673943375
5. https://tracehound-tau.vercel.app/benchmark
6. `tracehound-09-seeded-test.jpg`, opened full screen (drag it into a Chrome tab, or Preview full screen)
7. https://tracehound-tau.vercel.app/graph (opens on Recall)
8. https://github.com/SunnyBagal/TraceHound#try-it
9. End card (slide or image; text in shot 8)

**Graph tab prepared**
- [ ] Open the **PULL REQUESTS** lane (top left) and scroll it so **#4 "Return the existing card when a saved link is added…"** (1 finding) is in view.
- [ ] Click #4 once to check the changed component lights up, then reset with **× Plain graph**, so the real click happens on camera.

**Never on screen**
- Any competitor name. The README names one in a bullet just above "## Try it" and several under "## Results": open the `#try-it` anchor and check that bullet is scrolled out of frame.
- The Claude avatar on the commit row of PR #4 and PR #7 (a third-party mark): keep it out of every zoomed frame.
- The tracehound-demo PR #2 review (older Nano generators, not the current config; numbers §7).

## Shots

| # | time | tab / URL | actions, in order | zoom | overlay | must be visible |
|---|---|---|---|---|---|---|
| 1 | 0:00–0:10 | Opening card | Hold, no movement. | none | Line 1: "Third-party logos are blurred and the five commercial reviewers are shown as Reviewer A–E." Line 2: "TraceHound's reviewer runs only on NVIDIA Nemotron, via Nebius Token Factory." | Both lines, readable for the full 10 s. |
| 2 | 0:10–0:32 | `tracehound-tau.vercel.app` | Whole page, top to bottom. 0:10 hold on the hero ("Understand your code. Fix what breaks.") until "Nebius Token Factory" is said. 0:15 scroll through **"The graph is the review surface"** and **"Built to be checked."** without stopping. 0:20 **stop** on **"Understand what agents write. Don't read 2,040 lines. Read 8."** with the 8-declarations panel in view, while the voice says "the few declarations that actually changed". 0:25 scroll on through **"How it works"** (the plates) as the voice says "maps your repository", then **"Agents"**. 0:30 land on the closing **"See your pull requests on the map."** | none | none | The hero subtitle naming NVIDIA Nemotron and Nebius Token Factory (0:10–0:15); the "8 declarations" panel (0:20–0:25). |
| 3 | 0:32–0:59 | `recall-seeded/pull/4` | 0:32 PR title "Return the existing card when a saved link is added again #4" in view. 0:34 scroll to the github-actions review. 0:36 zoom on the review header, hold through "a third pass checks each finding". 0:48 zoom out, scroll to the line comment on `recall-backend/index.ts`, lines 127–130. 0:50 zoom on the comment text, hold to the end. | 0:36 header; 0:50 code lines + comment | none | **`TraceHound · pool-verify-v1 · nvidia/Nemotron-3.5-Super-VL`** (0:36–0:48); the four `+` lines and the first two sentences of the comment (0:50–0:59). |
| 4 | 0:59–1:12 | `recall-seeded/pull/7#pullrequestreview-…` | 0:59 the page opens at the review. 1:00 zoom on the header, then glide down to **"Files you didn't edit that depend on this change"** and hold on both bullets. | 1:00 header → section | 1:01–1:12, lower third: "Deterministic contract check · no model" | Header `0 findings (pool 0, kept 0)`; both bullets: `bench-search.ts:330` and `useSearch.ts:8`, each saying the file still uses `/api/v1/search`, renamed to `/api/v1/content/search`. |
| 5 | 1:12–1:49 | `/benchmark` | Whole page, top to bottom, with **four stops**. 1:12 **stop 1**: the top, the headline "Tied with four of five commercial reviewers on the blind test, ahead of one. Every loss is on this page." and the 57.1% tile. 1:17 scroll to **Blind test → F by reviewer**; **stop 2** with the whole chart and its footnote in view (1:19–1:28); hover the TraceHound v2 bar. 1:28 scroll into the light **"Dev and blind sets together"**; **stop 3** on its bar chart (1:30–1:37). 1:37 scroll steadily past the tables, **"Dev set"** and **"Other runs"**, without stopping. 1:42 **stop 4**: the **Cost** footnote ("about $0.06–0.07 per pull request on 3.5, about 2 minutes") while the voice says the cost, then scroll to **Limits** with the wolves and hold to the end. | 1:20 push-in on the blind chart | none | Stop 1: "57.1%" and "point estimate". Stop 2: seven bars labelled Reviewer A–E / TraceHound v1 / v2, the "TIED WITH V2" band, the footnote "statistically tied with … ahead of Reviewer E". Stop 3: the caption "v2 was tuned on the dev set". Stop 4: the cost sentence; the first Limits bullet ("10 pull requests and 23 golden findings … within noise"). |
| 6 | 1:49–2:13 | Seeded-test card (`tracehound-09-seeded-test.jpg`) | Hold the card. 1:49 full card. 1:56 slow push-in to the **dot rows**, following the voice: "Inside the diff" row, then the three cross-file rows. 2:06 pull back to the bold line under the panel. | slow push-in, then out | none (the card is the overlay) | Headline: "9 of 22 bugs caught, 0 of 8 clean PRs flagged"; "Pre-registered on 19 Oct"; the green 0 / 8 row; **"3 of the 4 cross-file catches came from the deterministic contract check, not the model."** (2:06–2:13). |
| 7 | 2:13–2:38 | `/graph` (Recall) | 2:13 hold on the plain Recall graph. 2:24 click **#4** in the PR lane on "Pick a pull request": changed components light up, the rest dim. 2:30 click the lit **Recall API Service** node, open **Connections**, click one connection (e.g. *produces → Content Processing Queue*). Hold on its evidence. | 2:32 the sidebar's evidence | none | The PR lane with #4 selected and "1 finding"; the lit component; one connection's **file, symbol and line range** (2:33–2:38). |
| 8 | 2:38–2:50 | `github.com/SunnyBagal/TraceHound#try-it`, then end card | 2:38 the "Try it" section with the YAML block in view. 2:40 zoom on the last lines: `nebius-api-key: ${{ secrets.NEBIUS_API_KEY }}` and `model: 'nvidia/Nemotron-3.5-Super-VL'`. 2:46 cut to the end card, hold to 2:50. | 2:40 the `with:` block | End card: "TraceHound · github.com/SunnyBagal/TraceHound · tracehound-tau.vercel.app · MIT licensed · NVIDIA Nemotron via Nebius Token Factory" | `.github/workflows/tracehound-review.yml` above the block; the secret line and the model line. |

## Every spoken claim and its source

| shot | claim | source |
|---|---|---|
| 2 | Reviews with Nemotron 3.5 Super via Nebius Token Factory | numbers §0.5 |
| 2 | Cuts a change down to the declarations that changed | homepage "Diff → declarations"; change sets (CLAUDE.md). No number is spoken. |
| 2 | Every connection points to a file and a line | product rule (CLAUDE.md); homepage "Built to be checked" |
| 3 | Diff reviewer, context reviewer, a checking pass before posting | devpost-about "How we built it" (pool-verify-v1) |
| 3 | A planted bug in a copy of the owner's app; a race | numbers §7 (model catch, recall-seeded #4) |
| 4 | Deterministic contract check, no model; renamed route still used in two untouched files | numbers §7 (recall-seeded #7); the review lists two files |
| 5 | Locked in first, 10 blind PRs, one run, no tuning after; F 57.1% | numbers §1 |
| 5 | Statistically tied with four of five commercial reviewers, ahead of one | numbers §1 allowed sentence, §0.2 |
| 5 | All 50, 40 of them tuned on: behind the leader, tied with the next two, ahead of the last two | numbers §2 allowed sentence, §0.6 |
| 5 | About six to seven cents a pull request | numbers §8 |
| 6 | 30 PRs: 22 seeded + 8 clean; 9 of 22; silent on all 8; 5 of 6 in-diff; 4 of 16 cross-file; 3 of those 4 from the contract check, not the model | numbers §7 allowed sentence |
| 7 | The graph didn't help the repair agent; published | numbers §6 (decision 050) |
| 8 | One workflow file plus the key as a secret | README "Try it" |

## The seeded-test card

`tracehound-09-seeded-test.jpg` (2400×1600, same style as the gallery). Every number is from numbers.md §7 and §8; class definitions are from `tracehound-seeds/README.md`. It doubles as a gallery image: put it right after `tracehound-04-blind-test.jpg`. Devpost caption: "Pre-registered seeded test on two of my own apps: bugs, answer key and scorer committed before any PR. No other reviewer was run."

## After recording

- Total length ≤ 2:50. If over, use the cut order at the bottom of `video-voiceover.md`.
- Watch it once muted, checking for names, unblurred logos, the Claude avatar and tab hover cards.
- Upload to YouTube as **Public** (Devpost needs public, not unlisted). Title: "TraceHound: PR review on NVIDIA Nemotron via Nebius Token Factory". Send the URL to the chat.
