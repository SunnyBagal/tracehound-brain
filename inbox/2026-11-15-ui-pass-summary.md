# UI pass summary, 2026-11-15 (UI lead, raw)

Herdr space "UI" holds four panes: the UI lead (t1p), Bunny-UI (t1r), Codex-UI (t1s) and Grok-UI (t1t).

BASE is `main`. Vercel production builds from `main` (latest production deploy 66f52a0), and `ui/v2` is already merged into it. Every branch was cut from `origin/main` @ 66f52a0, in its own worktree under `~/Projects/th-ui/`.

Nothing is merged. The lead waits for Sunny.

| lane | PR | head | builder | reviewer | verdict | CI |
|---|---|---|---|---|---|---|
| ui-02 width + hero | #74 | 9e35223 | Codex-UI | Bunny-UI | **GO 9e35223** | green |
| ui-03 pricing | #73 | 309f9b3 | UI lead | Codex-UI | **NO-GO**: `Landing.tsx` +3/-0 vs "exactly one line" | green |
| ui-01 graph readability | #75 | 31ae6f5 | Bunny-UI | Codex-UI | **NO-GO**: two P2s in `lib/inspector-width.ts` | green (Bugbot neutral) |

## Open items
- **ui-03:** the rule says one line, but TSX needs the import and the nav array lives in `Landing.tsx`, so the minimum is 2 lines, or 3 with the nav entry. Sunny decides: amend the rule, or drop the nav entry or move NAV.
- **ui-01 P2 (a):** at 768–859px with no stored width, the default 480 exceeds the resize max of 400/420. The first arrow key shrinks the panel.
- **ui-01 P2 (b):** at 1280px, End followed by expanding the PR rail leaves about 8px of canvas, with the controls covered. Both were confirmed in headless Chrome.
- **ui-01, pointer drag:** not verified in headless Chrome (looks like a CDP pointer-capture artefact). It needs a real-browser check.
- Hero integration tiles include a Cursor logo (`Hero.tsx:33`). That is a product some readers see as a competitor; it isn't in any lane.
- Codex's sandbox couldn't build or serve, so the lead ran its builds and browser checks.
- Grok: CodeRabbit $24/$48/$72 annual is confirmed ($30/$60/$90 monthly). Greptile's prepaid $1 credits with no seat fee is confirmed (22 Oct 2026). Suggested footer alternatives: "Price the seat, not the review." / "One seat price. Reviews stay included."

## Before / after (1440px; images beside this note in `2026-11-15-ui-pass/`)
- Graph detail: `graph-before.png` → `graph-after.png`. The finding text no longer appears three times. Now there is one clamped effect line, a collapsed Details toggle, and a count + `file:line` callout. The canvas is slate and the PR list folds to a rail.
- Hero: `hero-before.png` → `hero-after.png` ("Agents write the code." / *We catch what breaks.*; 1440px content width).
- Pricing: `pricing-before.png` (main's page bottom, no pricing) → `pricing-after.png`.

Raw reports from each lane: `2026-11-15-{bunny-ui-ui-01,codex-ui-ui-02,codex-ui-review,grok-ui-research,ui-lead-ui-03}.md`. None are committed: brain changes need a codex GO before push.

## Update: pricing moved to its own page (Sunny's ask)
- ui-04 is PR #76 @ 4b09a73 (Codex-UI): a static `/pricing` page in the Reviews/Benchmark shell (`ReviewShell` + `ShellHero`) with the same plans. The landing page has no pricing section, only a nav link (`Landing.tsx` +1). PR #73 is closed as superseded.
- Review by Bunny-UI: **GO 4b09a73**, with two P2s outside the checked widths, each needing CSS outside the lane:
  - at 320px the 4th shell tab is cut off ("Pricin")
  - the landing nav overlap, already on main at 864–988px, now reaches 1060px, including 1024px
- The P3s: phone min-height gap, no hero art panel, sr-only price text can drift.
- Screenshots: `~/Projects/th-ui/shots/ui-04/` and `shots/review-bunny/ui-04/`.

## Merged (Sunny: "go merge them all", 2026-11-15)
Merge commits on `main`, in order. Nothing was force-pushed: later branches got `main` merged in, then gates and CI were rerun.
- 31409df: ui-02 #74 at 9e35223 (bunny GO).
- ca325f1: ui-04 #76 at 62b51c5 (bunny GO on 4b09a73, plus main merged in; 218 tests).
- 9bf04b2: ui-01 #75 at 217f2e4. Codex gave NO-GO on 31ae6f5; it was merged on Sunny's ruling (225 tests).
- #73 (ui-03) was closed, superseded by #76.

Follow-ups still open:
- ui-01: the inspector width bounds at 768–859px; at 1280px, End + expanded PR rail covers the canvas; pointer drag needs a real-browser check.
- ui-04: the 4th shell tab is cut off at 320px; the landing nav overlap now reaches 864–1060px.
- The hero's Cursor tile.
