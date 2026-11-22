# bunny promo-review, stand-in for grok (raw, verbatim from tracehound-testset/run/out/bunny.promo-review.md)

GO b61b85fe2ea4459ea3d61897035651c14f315975

# bunny.promo-review: honesty, phone readability and spec of the promo race video (stand-in for grok)

**Scope:**
- I reviewed `promo/race/` @ b61b85f from my own detached worktree `~/Projects/TraceHound-bunny-promo`. I made no edits.
- The page is `docs/bench/ts-heldout.md` on origin/main 6d8c543, unchanged since 890943e.
- I opened no held-out folder. $0.
- The numbers themselves were checked separately (`bunny.promo-numbers.md`, PASS).

**Verdict:**
- Nothing on screen is false.
- No frame shows a winner, a place, a trophy or a finish.
- The three short phrases are supported by the page.
- The fixes below are recommendations. None blocks.
- The post must carry what the video leaves out (§1d).

## 1. Honesty

**a. "Won" / "finished".**
- No frame names a winner or a place.
- The only finish language is "Nobody finished.", which is true: the best is 12 of 20.
- The lanes are ordered by count (Greptile, CodeRabbit, TraceHound), so the video does imply a ranking of the point estimates.
  - That order is the page's own §2 order by catches. It is not wrong, and TraceHound, ours, is last.
  - But only one of the three gaps is a tested, clear difference. That point goes in the post (d).

**b. Does the even pace imply a per-PR order or timing?**
- No:
  - every racer's ticks are spread evenly;
  - all last ticks land at the same frame (183);
  - there is no PR counter;
  - the top line is plain progress.
- Nobody is shown to be faster, and no tick lines up with a pull request.
- A viewer could still read "the race" as speed. "Nobody finished." and the scoreboard heading "Bugs caught, of 20" pull it back to counts. This is acceptable, and I suggest no change.

**c. The three phrases against the page's wording.**

| phrase | page | change? |
|---|---|---|
| "20 real bugs" | §1 line 11: "20 of which introduced a bug that a later merged fix repaired"; "labelled bugs" throughout. These are real upstream bugs, not seeded ones. | **No change.** "Real" is fair against seeded benchmarks. Limit 3 ("clean means no qualifying later fix was found, not bug-free") is a post-level nuance. |
| "hand-graded blind" | Line 42 "Catches are hand-checked (blind to the tool)"; §5 line 116 "Blind hand-check by the project owner, with the tool hidden, the judges' verdicts hidden, the order shuffled…"; limit 5: the checker's tool guesses were right for 25 of 61, the same as always naming the commonest tool. | **No change.** It is supported, and the 25 of 61 result backs "blind". "Graded" slightly overstates what was done: a human confirmed or rejected the matches the judges proposed, on a 61-item sheet. That is fine at slogan length. |
| "Pre-registered" | Line 3: "Predictions and claim wording: decision 078, registered before the first held-out pull request"; protocol revision 13; "registered tests" (§1, §3). | **No change.** "Pre-registered" is the standard term for exactly this. The page's "registered before …" carries the same meaning. My earlier note was about literal wording only. |

**d. What a reader needs that the video does not show.**
- **"One run".** The page puts it in the headline sentences (§1 line 31). It is the most important missing qualifier, and costs about 9 characters.
  - **Recommended in the video:** footer "Pre-registered · hand-graded blind · one run · PRs from medusa, payload, twenty, bullmq". If the footer gets too long, drop "PRs from" and keep the four names.
  - If it stays out of the video, it must be in the post.
- **Significance, for the post only.**
  - TraceHound vs Greptile: 0 vs 8 one-sided misses, exact sign test p = 0.0078. This is a clear difference.
  - TraceHound vs CodeRabbit: 2 vs 8, p = 0.109. This is not distinguishable at 20 bugs.
  - Greptile vs CodeRabbit was not tested.
  - The Wilson intervals overlap widely: 8.1–41.6%, 29.9–70.1%, 38.7–78.1%.
  - Too dense for a 9 s loop.
- **Free plans, for the post only.** CodeRabbit and Greptile ran on free plans, one pull request every 61 minutes (limit 7; table "free tier"). Readers will assume paid tiers otherwise.
- **Who graded, for the post (optional).** The hand-check was by the project owner, TraceHound's author, blind to the tool (limit 5). Saying so pre-empts the obvious objection.
- **Optional, for the post.** TraceHound's own registered prediction was 8 (range 5–11), and the result of 4 was outside it (§8 row 1). Saying so is a credible honesty signal.

**e. Imagery.** The tiles carry text names only (no logo assets on the branch), with no mascot and no drawn logo, as per spec. If official logos are added later (README §Logos), re-check that none is altered.

## 2. Readability at phone size
I made a 640 px copy (`ffmpeg -vf scale=640:-2`, 640×360) in `/private/tmp/bunny-promo/phone/` and read frames at 0.5, 3.5, 6.5 and 8.0 s. A 640 px copy is still larger than a phone timeline, where the video is about 360–390 pt wide. So anything marginal at 640 px is worse in the feed.

| element | source size | at 640 px | reading |
|---|---|---|---|
| title | 62 px bold | ~21 px | fine |
| live counts / scoreboard numbers | 72 / 64 px bold | ~24 / ~21 px | fine |
| "Nobody finished." | 64 px bold | ~21 px | readable, but **short on screen**: it fades in over 186–200 and the scoreboard covers it from 210, so it is at full opacity for about 0.3 s (≈ 0.8 s visible). At 6.5 s it is still mid-fade (grey). |
| tile names / scoreboard names | 38 / 52 px semibold | ~13 / ~17 px | fine |
| "20 bugs" label at the finish | 40 px, dim #a1a1aa | ~13 px grey | small but readable |
| **footer** | 40 px regular, dim #a1a1aa | ~13 px thin grey | **too small and too faint for a phone feed.** It holds the three honesty phrases, so it should be the most legible small text |
| track lines and tick marks | 2 px at 10–16% white | ~0.7 px | effectively invisible. They are decorative, which is fine |
| scoreboard bar track ("of 20" scale) | 8 px at 10% white | ~2.7 px | barely visible. The fills read, but the empty part that shows "out of 20" nearly disappears |

## 3. Spec

| item | measured | ok |
|---|---|---|
| size | 1920×1080 | yes |
| frame rate | 30/1, 270 frames, 9.000 s | yes |
| codec | H.264 High, yuv420p | yes |
| faststart | top-level atoms ftyp, moov @ 32, free, mdat @ 4045 (moov before mdat) | yes |
| file size | 550,676 bytes (0.55 MB) | yes (< 15 MB) |
| audio | none (video stream only) | fine for X autoplay (muted anyway) |
| loop | The scoreboard fades out over 255–270 onto the track in its title state. Frame 269 vs frame 0 has mean abs diff 1.18 (frame 1 vs 0: 0.01), with the scoreboard at 1/15 opacity. Frame 270 = frame 0 | yes, seamless |

## Fixes (recommended, none blocking)
1. Footer: add "one run". If space is short, drop "PRs from".
2. Footer: raise it from 40 px to about 48–52 px and from #a1a1aa to about #d4d4d8 (or C.text), so it survives the phone feed.
3. Hold "Nobody finished." longer: for example, finish its fade by about frame 194 and start the scoreboard fade at about 222 instead of 210. That gives about 1 s at full opacity; shift SCORE_SOLID, RESET and LOOP_OUT to match, or trim the scoreboard hold.
4. Scoreboard: bar track from 10% to about 18–20% white, and bar height from 8 px to about 12 px, so "of 20" reads on a phone.
5. The post must carry these (not the video):
   - one run (if not in the footer);
   - TraceHound vs Greptile p = 0.0078 (clear), and TraceHound vs CodeRabbit p = 0.109 (not distinguishable);
   - CodeRabbit and Greptile on free plans;
   - a link to `docs/bench/ts-heldout.md`.

   Optional:
   - grading by the project owner, blind to the tool;
   - TraceHound's own prediction (8) missed.
