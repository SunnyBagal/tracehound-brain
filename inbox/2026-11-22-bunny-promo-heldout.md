# bunny promo-heldout (raw, verbatim from tracehound-testset/run/out/bunny.promo-heldout.md)

PASS 91b243c3b3a83508bce6c2ea033bd984efad8869 · brain GO (NOW.md, timeline.md, inbox/2026-11-22-artu1-promo-heldout.md, plus my inbox/2026-11-22-bunny-promo-heldout.md)

# bunny.promo-heldout: HyperFrames bar chart number check + brain GO

**What I checked:**
- **Video:** `~/Projects/TraceHound-promo` at `91b243c3b3a83508bce6c2ea033bd984efad8869`, read only. It equals origin `promo/race-animation` and PR #79's head (OPEN). I made no edits; the worktree's untracked `promo/race/` is not mine and I left it alone.
- **Page:** `~/Projects/TraceHound/docs/bench/ts-heldout.md` (last changed in `687e58a`).
- **Boundaries:** no held-out folder opened, no model call. $0.
- **Tools:** ffprobe/ffmpeg, frames in `/private/tmp/bunny-promo/heldout/`.

## 1. Numbers: PASS
Every on-screen string comes from `data.json` (`index.html` adds only the tick numbers and the animated counts).

| on screen | page | verdict |
|---|---|---|
| Title "AI Code Review Benchmark" | n/a (title) | fine |
| Badge "Bugs caught, of 20" | l.45 header "bugs caught of 20, hand-checked" | right |
| Subtitle "On a pre-registered blind test of fresh pull requests, TraceHound caught the fewest bugs of 3 AI code reviewers" | l.11 "29 fresh pull requests"; registered (078); l.15/116 blind hand-check; 4 < 10 < 12 (l.47–49) | right; a count, not a significance claim, and it understates TraceHound rather than overclaims |
| Caption "29 fresh TypeScript pull requests (20 real bugs), hand-checked blind, one run" | l.11, l.42 "29 pull requests (20 bug, 9 clean), one run" | right ("real bug" = one a later merged fix repaired, l.11; the brain's NOW uses the same words) |
| Scores Greptile 12, CodeRabbit 10, TraceHound 4 | l.47–49, l.84–86 | right; bars 12/10/4 segments on a 0–20 axis |
| "Most bugs caught" on Greptile | 12 is the highest count | right; not "winner", and Greptile vs CodeRabbit is never claimed |
| Dashed line at 4 labelled "TraceHound" | 4 | right |
| Subtexts "PR review" ×2, "Nemotron 3.5 Super" + NVIDIA mark | l.35 "Nemotron 3.5 on Nebius", l.91 `nvidia/Nemotron-3.5-Super-VL` | right |
| Footer "held-out, pre-registered · checked by hand, tool hidden · PRs from medusa, payload, twenty, bullmq" | l.11, l.15, l.116 | right |
| Footer "TraceHound vs Greptile p = 0.008 · vs CodeRabbit p = 0.11 (not significant)" | l.69–70: 0.0078125 → 0.008, 0.109375 → 0.11; l.20/73 "no detectable difference" | right |
| Footer "CodeRabbit and Greptile on free plans" | l.48–49 "free tier", limit 7 (l.290) | right |
| Footer "Results run by Sunny Bagal" | owner did the hand-check (l.116, limit 5); grok operated collection/H2 (l.242) | acceptable as the project's byline |

Nothing overclaims: no "winner", no significance beyond the two stated p values, "fewest of 3" is a plain count.

**Motion:** the order Greptile, CodeRabbit, TraceHound never changes (CodeRabbit starts earlier, eases faster and ends higher than TraceHound; Greptile/CodeRabbit share timing, so 12:10 holds throughout), so no slot swaps fire. Counts rise from 0 to the total, monotone. Frame 75 (2.5 s) shows 12 / 10 / 3; frame 209 shows the final card.

**Spec:** 1920×1080, H.264 High, yuv420p, 30 fps, 210 frames, 7.000 s, 254,744 bytes, faststart (moov at 36, mdat at 1719). `-first.png` is pixel-identical to frame 0 and `-final.png` to frame 195 (PSNR inf); sanity: first vs frame 195 is 22.2 dB.

**Non-blocking:**
- N1. Frame 0 (the X thumbnail) shows **0 / 0 / 0** under "Bugs caught, of 20". With autoplay off, or in a reposted still, that reads as "nobody caught anything". Hide the counts until bars start (or show "–"), or upload `-final.png` as the post's thumbnail.
- N2. The code comment "the highlight … so it overtakes late" (`index.html`) is stale: with these totals nothing overtakes. Harmless.
- N3. `promo/heldout/README.md` names a local path (`~/Projects/tracehound-video/bench`) in a public repo. Harmless, but "Sunny's October video" alone would do.
- N4. Footer line 1 ends with a trailing "·".

## 2. Brain: GO
Reviewed the uncommitted `git diff NOW.md timeline.md` and `inbox/2026-11-22-artu1-promo-heldout.md` in `~/Projects/tracehound-brain` (HEAD `80323cc`).
- **Public-safe:** totals, p values, repo names and file paths only. No case, PR number, bug text or per-PR data.
- **Accurate:** 12 / 10 / 4 of 20, p = 0.008 / 0.11, free plans, 29 PRs / 20 bugs, 7 s / 210 frames / 254,744 bytes / faststart all match the video and the page. "Most bugs caught, not winner" matches the video.
- **Sha:** `91b243c` is the branch and PR #79 head; PR #79 is open (not merged), as written. `promo/race/` is removed in that commit, as written.
- **Nit (not blocking):** artu1's note says "Frame 0 shows the full card (the X thumbnail)"; it does, with counts at 0 (see N1).
- NOW.md:37 now says PR #77 merged (`bdc7ed4`), which fixes the stale line I flagged last time.
- My number check is written to the brain as `inbox/2026-11-22-bunny-promo-heldout.md` (not committed, not pushed).
