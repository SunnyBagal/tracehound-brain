# bunny promo-numbers (raw, verbatim from tracehound-testset/run/out/bunny.promo-numbers.md)

PASS. Every number on screen equals `docs/bench/ts-heldout.md`. The counts rise only by one, the bars are exact, and the stills match frames 0 and 240.

# bunny.promo-numbers: promo race video vs the published page

**What I checked:**
- **Video:** branch `promo/race-animation` @ `b61b85fe2ea4459ea3d61897035651c14f315975`, from my own detached worktree `~/Projects/TraceHound-bunny-promo`. I made no edits to the branch.
- **MP4:** `promo/race/renders/tracehound-race.mp4` (sha256 `d886ca0eff9e5998…`). It is h264 1920×1080, 30/1 fps, 270 frames, 9.000 s, as briefed.
- **Page:** `docs/bench/ts-heldout.md` at origin/main `6d8c543`. It is unchanged since `890943e` (`git diff --stat` is empty).
- **Boundaries:** I opened no held-out folder and not tracehound-bench. $0.
- **Tools:** ffmpeg, plus two stdlib-only Python scripts in `/private/tmp/bunny-promo/scripts/`. The frames are in `/private/tmp/bunny-promo/frames/`.

## 1–2. Frames read, and every number against the page
| t (s) | on screen |
|---|---|
| 0.5 | Title "29 fresh TypeScript PRs. 20 real bugs. 3 AI reviewers."; "20 bugs" at the finish line; counts 0 / 0 / 0 |
| 1.5 | Race begins; counts 0 / 0 / 0 (the first change is at frame 46 = 1.53 s) |
| 3.5 | Greptile 5, CodeRabbit 4, TraceHound 1 |
| 6.0 | 11 / 9 / 3 |
| 6.5 | "Nobody finished."; 12 / 10 / 4; tiles' right edges at x ≈ 1127 / 1009 / 655 |
| 7.5 | Scoreboard "Bugs caught, of 20": Greptile 12, CodeRabbit 10, TraceHound 4 (bars still easing in); footer "Pre-registered · hand-graded blind · PRs from medusa, payload, twenty, bullmq" |
| 8.0 | The same scoreboard, bars at final length |
| 8.95 | Fade back to the title state (counts 0), with the scoreboard fading out over it. This is the designed loop: frame 270 = frame 0 |

The tile positions are `xAt(n)` = 420 + n/20 × 1180: 1128 / 1010 / 656.

Page sources:
- **29 PRs:** §1 line 11 ("On 29 fresh pull requests…"); §2 line 42 ("29 pull requests (20 bug, 9 clean)").
- **20 bugs and "of 20":** line 11 ("20 of which introduced a bug"); the line 45 header "bugs caught of 20, hand-checked".
- **Greptile 12, CodeRabbit 10, TraceHound 4:** §1 lines 12–14; §2 lines 47–49, column "bugs caught of 20, hand-checked" (4 / 10 / 12).
- **Four repositories:** line 11, "medusa (8), payload (10), twenty (10), and bullmq (1)".
- **"3 AI reviewers":** the three tools of the page's title and table.

`promo/race/src/data.ts` cites "§1 and §2" for 29/20, "§1 and §2, 'bugs caught of 20, hand-checked'" for the counts, and "§1" for the repositories, with main 890943e. All of these are right: the quoted column header is verbatim at line 45. It gives section numbers, not line numbers.

Wording (not numbers, for artu1's judgment; no change made):
- **"Pre-registered":** the page says "registered" (§8 "registered before the window", decision 078; line 151 "registered (pre-specified)"). It never writes "pre-registered". The claim is supported, but not verbatim.
- **"hand-graded blind":** the page says "hand-checked (blind to the tool)" (line 42) and "Blind hand-check" (line 116). This is supported.
- **"20 real bugs":** the page says "introduced a bug that a later merged fix repaired" (line 11). "Real" is the video's word.

## 3. Live counts over all 270 frames
I evaluated the source's `count(f, n)` (Race.tsx, a tick counts once at least half done) together with timing.ts (TITLE_END 36, RACE_END 186, TICK 6; `trackFrame` = 0 from RESET 225) for every frame.
- Every frame-to-frame step is 0 or +1.
- The maximum is 12 / 10 / 4.
- From frame 186 (6.2 s) to 224 the counts are exactly 12 / 10 / 4.
- Change frames:
  - Greptile 46, 58, 71, 83, 96, 108, 121, 133, 146, 158, 171, 183 (12);
  - CodeRabbit 48, 63, 78, 93, 108, 123, 138, 153, 168, 183 (10);
  - TraceHound 71, 108, 146, 183 (4).
  - The last tick of each lands at 183 ≤ 186.

**I checked these against the pixels** by decoding all 270 frames and diffing each lane's count box (x 1640–1840, y ±45) frame to frame over frames 1–209.
- The changed frames equal the model's list exactly, for all three lanes.
- The noise on unchanged frames is at most 1,095; the smallest real change is 85,509.

The frames read at 0.5 / 3.5 / 6.0 / 6.5 s (0/0/0, 5/4/1, 11/9/3, 12/10/4) agree with the model at frames 15 / 105 / 180 / 195.

## 4. Scoreboard bars, frame 240 (8.0 s)
These are the pixel rows at bar centre y = 430 / 615 / 800.

| bar | fill (blue→teal) | track right end | fill / track | caught/20 |
|---|---|---|---|---|
| Greptile | x 460–1059 = 600 px | 1459 | 600 / 1000 = 0.600 | 12/20 = 0.600 |
| CodeRabbit | 460–959 = 500 px | 1459 | 500 / 1000 = 0.500 | 10/20 = 0.500 |
| TraceHound | 460–659 = 200 px | 1459 | 200 / 1000 = 0.200 | 4/20 = 0.200 |

The track runs 460–1459 = 1000 px, which is BAR_W in Race.tsx. The few lit pixels at x 456–459 are the fill's 12 px glow (box-shadow), not track.

## 5. Stills
I compared each still to the decoded frames by mean absolute difference per channel (0–255, every 97th byte). Both stills are 1920×1080.

| still | vs its frame | vs the other frame |
|---|---|---|
| `renders/tracehound-race-first.png` | frame 0: 1.07 | frame 240: 11.51 |
| `renders/tracehound-race-scoreboard.png` | frame 240: 1.11 | frame 0: 11.53 |

A difference of about 1 is h264 compression. Each still is the frame it should be.
