# bunny promo-recheck (raw, verbatim from tracehound-testset/run/out/bunny.promo-recheck.md)

GO 8cd38b3f9a3cfbe5c2b57e11b8daaeae0640a7b8

# bunny.promo-recheck: promo video after the four review fixes

**Scope:**
- `promo/race/` @ 8cd38b3 (parent b61b85f), from my detached worktree `~/Projects/TraceHound-bunny-promo`. I made no edits.
- The diff from b61b85f is `Race.tsx`, `data.ts`, `timing.ts` and `render.mjs`, plus the MP4 and the scoreboard still. The first still is byte-unchanged, as frame 0 is unchanged.
- No held-out folder was opened. $0.
- The scripts are the same as for `bunny.promo-numbers.md`; the only change is the scoreboard timing. They are in `/private/tmp/bunny-promo/scripts/` (`counts2.py`, `check2.py`).
- New MP4: sha256 `f9c60cd925c362b9…`.

## 1. Numbers, again on the new MP4
- **Live counts.**
  - All 270 frames are decoded.
  - The frames where each count box changes, over frames 1–217 (up to SCORE_IN 218), equal the source model exactly:
    - Greptile 46…183 (12 changes);
    - CodeRabbit 48…183 (10);
    - TraceHound 71, 108, 146, 183 (4).
  - Each step is +1, the maximum is 12 / 10 / 4, and the counts are exactly 12 / 10 / 4 from frame 186 to the scoreboard.
  - The noise on unchanged frames is at most 1,173; the smallest real change is 85,509.
- **Bars at frame 240.**
  - The fill finishes by frame 236: the source fill ends at SCORE_SOLID+4+2i = 232, 234 and 236.
  - The fills are x 460–1059 / 460–959 / 460–659 = **600 / 500 / 200 px** on a track running 460–1459 = 1000 px, which is exactly 12/20, 10/20 and 4/20.
  - The lit pixels at 456–459 are the fill's glow.
- **Stills.**
  - `tracehound-race-first.png` against frame 0 has a mean abs diff of 1.07 (13.48 against frame 240).
  - `tracehound-race-scoreboard.png` against frame 240 has 1.12 (13.55 against frame 0).
  - Each is its frame, apart from h264 noise.
- **Wording.** "one run" is supported by §1 line 31 ("One run, 20 bugs, …"), which `data.ts` now cites. The other words are unchanged and were already supported (`bunny.promo-review.md` §1c).

## 2. The four fixes
1. **Footer.**
   - It has two lines: "Pre-registered · hand-graded blind · one run" / "PRs from medusa, payload, twenty, bullmq".
   - It is 48 px, weight 500, #d4d4d8. The peak R on the frame-240 footer measures 219 (expected 212), against 161 for the old #a1a1aa.
   - At 640 px both lines read clearly. It is now the most legible small text on the scoreboard, and it fits above the bottom edge.
2. **"Nobody finished."**
   - The peak brightness of its text rises over frames 186–193 and is at full (233) from frame 194.
   - It stays clear until the scoreboard starts fading in at 218, which is 24 frames = 0.8 s at full opacity (was about 0.3 s).
   - The 640 px frames at 6.5 s and 7.0 s both show it solid and white.
3. **Bar track.**
   - It is 12 px tall at 20% white.
   - At 1920 and at 640 px the empty part of each bar is clearly visible, so "of 20" reads.
   - The fills keep their exact lengths (above).
4. **Timing.**
   - Scoreboard: fades in 218–228, then solid; the track resets under it at RESET 231; fades out 258–270.
   - This is consistent: RESET > SCORE_SOLID, so the reset is never visible.
   - Fully solid scoreboard hold: 228–258, 1.0 s (was 1.1 s). It is still enough to read three rows and the footer.

## 3. Spec and loop

| item | measured |
|---|---|
| size / fps / frames | 1920×1080, 30/1, 270 frames, 9.000 s |
| codec | H.264 High, yuv420p, video only |
| faststart | ftyp, moov @ 32, free, mdat @ 4045 (moov before mdat) |
| file size | 539,180 bytes (< 15 MB) |
| loop | The fade-out over 258–270 lands on the title state. Against frame 0, frame 258 is 12.68, 264 is 6.42, 268 is 2.58 and 269 is 1.56, and frame 270 = frame 0. It is seamless |

Nothing blocks. The post still has to carry the items in `bunny.promo-review.md` §1d:
- TraceHound vs Greptile p = 0.0078, and TraceHound vs CodeRabbit p = 0.109;
- that CodeRabbit and Greptile ran on free plans;
- a link to the page.
