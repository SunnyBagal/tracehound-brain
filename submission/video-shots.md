# TraceHound video: shot list (2:50)

What's on screen, shot by shot. The words are in `video-voiceover.md` (same shot numbers and times). Supersedes `video-script-v3.md`; same order: opening card → homepage → recall-seeded PR #4 → PR #7 → /benchmark → /graph with PR #4 → install. One cut is added after /benchmark: the seeded-test results file, because the seeded numbers are not on the site.

Arc: the problem (shot 1) → what TraceHound does (2) → a real catch (3, 4) → how we know (5, 6) → what didn't work (7) → install in one file (8).

## Before recording

**Site state**
- [ ] https://tracehound-tau.vercel.app/benchmark shows **Reviewer A–E** and blurred logos (`BLUR_THIRD_PARTY_MARKS = true`). Check the homepage hero too: every mark except NVIDIA's is blurred.
- [ ] Every page renders **dark**. If /benchmark shows a light section (seen in one screenshot), reload, and set macOS appearance to Dark.

**Browser**
- [ ] One Chrome window at **1920×1080**, browser zoom **100%** on every tab (⌘0). Do zooms in the editor or with the recorder's auto-zoom, never with browser zoom (it reflows the page).
- [ ] Bookmarks bar hidden (⌘⇧B), extension icons unpinned, no other windows visible.
- [ ] Never hover over a tab: Chrome's hover card ("Memory usage …") appeared over a screenshot before.
- [ ] Signed in to GitHub is fine; dark theme on GitHub.

**Mac**
- [ ] Do Not Disturb on, Slack/Discord/Mail closed, Dock hidden, menu bar clock hidden or ignored.
- [ ] Mic check: one 10-second test take, listen back.

**Tabs, in this order, each scrolled to the top**
1. Opening card (a full-screen slide or image; text in shot 1)
2. https://tracehound-tau.vercel.app
3. https://github.com/SunnyBagal/recall-seeded/pull/4
4. https://github.com/SunnyBagal/recall-seeded/pull/7#pullrequestreview-5673943375
5. https://tracehound-tau.vercel.app/benchmark
6. https://github.com/SunnyBagal/tracehound-seeds/blob/main/RESULTS.md
7. https://tracehound-tau.vercel.app/graph (Recall snapshot, `256513a`)
8. https://github.com/SunnyBagal/TraceHound#try-it
9. End card (slide or image; text in shot 8)

**Graph tab prepared**
- [ ] Open the **PULL REQUESTS** lane (top left) and scroll it so **#4 "Return the existing card when a saved link is added…"** (1 finding) is in view.
- [ ] Click #4 once to check the changed component lights up, then reset with **× Plain graph**, so the click happens on camera.

**Things that must never be on screen**
- Any competitor name. RESULTS.md names three reviewers under "Deviations", far down the file: stay above "## C controls". The README names them under "## Results", and one bullet just above "## Try it" (in "See it without a key") names one: open the `#try-it` anchor and check that bullet is scrolled out of frame before you record.
- The Claude avatar on the commit row of PR #4 and PR #7 (a third-party mark): keep it out of every zoomed frame.
- The tracehound-demo PR #2 review: it was made with older Nano generators, not the current config (numbers §7).

## Shots

| # | time | tab / URL | actions, in order | zoom | overlay | must be visible |
|---|---|---|---|---|---|---|
| 1 | 0:00–0:10 | Opening card | Hold, no movement. | none | Line 1: "Third-party logos are blurred and the five commercial reviewers are shown as Reviewer A–E." Line 2: "TraceHound's reviewer runs only on NVIDIA Nemotron, via Nebius Token Factory." | Both lines, readable for the full 10 s. |
| 2 | 0:10–0:29 | `tracehound-tau.vercel.app` | 0:10 hold on the hero (wolves, "Understand your code. Fix what breaks."). 0:15 scroll slowly to **"Understand what agents write. Don't read 2,040 lines. Read 8."**, hold 3 s. 0:22 scroll to **"Built to be checked."**, stop with the "No model draws the graph" card in view. | none | none | The hero subtitle naming NVIDIA Nemotron and Nebius Token Factory (0:10–0:15); "No model draws the graph" when the voice says "No model draws that map". |
| 3 | 0:29–0:57 | `recall-seeded/pull/4` | 0:29 PR title "Return the existing card when a saved link is added again #4" in view. 0:31 scroll to the github-actions review. 0:33 zoom on the review header, hold through "a third pass checks each finding". 0:45 zoom out, scroll to the line comment on `recall-backend/index.ts`, lines 127–130. 0:48 zoom on the comment text, hold to the end. | 0:33 header; 0:48 code lines + comment | none | The header **`TraceHound · pool-verify-v1 · nvidia/Nemotron-3.5-Super-VL · 1 findings`** (0:33–0:45); the four `+` lines and the comment's first two sentences on the non-atomic check (0:48–0:57). |
| 4 | 0:57–1:11 | `recall-seeded/pull/7#pullrequestreview-…` | 0:57 the page opens at the review. 0:58 zoom on the review header, then glide down to **"Files you didn't edit that depend on this change"** and hold on both bullets. | 0:58 header → section | 1:00–1:11, lower third: "Deterministic contract check · no model" | Header `0 findings (pool 0, kept 0)`; both bullets: `bench-search.ts:330` and `useSearch.ts:8`, each saying the file still uses `/api/v1/search`, renamed to `/api/v1/content/search`. |
| 5 | 1:11–1:41 | `/benchmark` | 1:11 hold on the top: "Tied with four of five commercial reviewers on the blind test, ahead of one. Every loss is on this page." and the 57.1% tile. 1:19 scroll to **Blind test → F by reviewer** (bench-judge-v1 tab), stop with the whole chart and its footnote in view. 1:22 hover the **TraceHound v2** bar. 1:33 scroll fast (or cut) to **Limits**, hold on the box with the wolves. | 1:24 chart, slight push-in | none | "57.1%" and "point estimate" (1:11–1:19); all seven bars labelled Reviewer A–E / TraceHound v1 / v2 and the "TIED WITH V2" band; the footnote "v2 is statistically tied with … ahead of Reviewer E"; the first Limits bullet ("10 pull requests and 23 golden findings … within noise") at 1:36–1:41. |
| 6 | 1:41–2:06 | `tracehound-seeds/…/RESULTS.md` | 1:41 hold on the title and the bold headline ("9 of 22 seeded defects caught, 0 false alarms on 8 controls"). 1:49 scroll to **"Caught / n (automatic rule)"**. 1:52 hold on the table. 2:00 scroll a few lines to the paragraph "Of the 4 cross-file catches, 3 … deterministic". Stop there. | 1:41 headline; 1:52 table rows | 1:41–2:06, lower third: "Pre-registered: seeds, key and scorer committed before any PR opened" | Headline 9/22 and 0 false alarms on 8; rows **D (in-diff) 5/6**, **all X (cross-file) 4/16**, **all seeded 9/22**; the sentence crediting 3 of 4 cross-file catches to the deterministic section (2:00–2:06). |
| 7 | 2:06–2:34 | `/graph` | 2:06 hold on the plain Recall graph. 2:16 click **#4** in the PR lane, on "Pick a pull request": changed components light up, the rest dim. 2:24 click the lit **Recall API Service** node, open the **Connections** tab, click one connection (e.g. *produces → Content Processing Queue*). Hold on its evidence. | 2:26 the sidebar's evidence | none | The PR lane with #4 selected and "1 finding"; the lit component; one connection's **file, symbol and line range** in the sidebar (2:28–2:34). |
| 8 | 2:34–2:50 | `github.com/SunnyBagal/TraceHound#try-it`, then end card | 2:34 the "Try it" section with the YAML block in view. 2:36 zoom on the last lines: `nebius-api-key: ${{ secrets.NEBIUS_API_KEY }}` and `model: 'nvidia/Nemotron-3.5-Super-VL'`. 2:45 cut to the end card, hold to 2:50. | 2:36 the `with:` block | End card: "TraceHound · github.com/SunnyBagal/TraceHound · tracehound-tau.vercel.app · MIT licensed · NVIDIA Nemotron via Nebius Token Factory" | `.github/workflows/tracehound-review.yml` in the sentence above the block; the secret and the model lines. |

## Every spoken claim and its source

| shot | claim | source |
|---|---|---|
| 2 | Reviews with Nemotron 3.5 Super via Nebius Token Factory | numbers §0.5 |
| 2 | No model draws the map; every connection points to a file and line | TraceHound product rule (CLAUDE.md); homepage "Built to be checked" |
| 3 | Diff reviewer, context reviewer, a checking pass before posting | devpost-about "How we built it" (pool-verify-v1) |
| 3 | A planted bug in a copy of the owner's app; race on the exact line | numbers §7 (model catch, recall-seeded #4) |
| 4 | Deterministic contract check, no model; renamed route still used in two untouched files | numbers §7 (recall-seeded #7); the review itself lists two files |
| 5 | Locked in before the run, 10 blind PRs, one run, no tuning after; F 57.1% | numbers §1 |
| 5 | Statistically tied with four of five commercial reviewers, ahead of one | numbers §1 allowed sentence, §0.2 |
| 6 | 30 PRs: 22 seeded + 8 clean; 9 of 22; silent on all 8; 5 of 6 in-diff; 4 of 16 cross-file; 3 of those 4 from the contract check | numbers §7 allowed sentence |
| 7 | The graph didn't help the repair agent; published | numbers §6 (decision 050) |
| 8 | About $0.06–0.07 per PR, pre-VAT | numbers §8 |
| 8 | One workflow file plus the key as a secret | README "Try it" |

## After recording

- Total length ≤ 2:50. If over, use the cut order at the bottom of `video-voiceover.md`.
- Watch it once muted, checking for names, unblurred logos, the Claude avatar and tab hover cards.
- Upload to YouTube as **Public** (Devpost needs public, not unlisted). Title: "TraceHound: PR review on NVIDIA Nemotron via Nebius Token Factory". Send the URL to the chat.
