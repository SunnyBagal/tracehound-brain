# video-01: the final video, as two files (shot list + voice-over)

Paste below the line into the **new planning chat** (the one started from `history/TraceHound_Handoff_Oct30.md`), after the handoff.

---

Write TraceHound's submission video as **two separate files** in the brain, then push them:

1. `submission/video-shots.md`, **what's on screen**: one row per shot with start–end time, the exact URL or tab, every click and scroll in order, where to zoom, any text overlay, and what must be visible (e.g. the review header showing `nvidia/Nemotron-3.5-Super-VL`). Add a "before recording" checklist: tabs to open, browser at 100%, notifications off, PR #4 selected in the PR lane, and so on.
2. `submission/video-voiceover.md`, **what Sunny says**: only the words to read, split by the same shot numbers and times. Write them to be spoken: short sentences, no brackets or symbols, numbers as they're said aloud. Add the word count per shot and the total.

Read first: the handoff, `submission/numbers.md` (all of it, especially the §0 wording rules), `submission/video-script-v3.md` (the current draft; keep its order: opening card → homepage → recall-seeded PR #4 → PR #7 → /benchmark → /graph with PR #4 → install), and `submission/devpost-about.md`.

Make it great, within these rules:
- **≤ 2:50 total.** At about 150 words a minute that's roughly 400 spoken words; stay at or under that.
- **A hook in the first 10 seconds:** the problem (AI agents write more PRs than anyone can read) and the promise, before any UI tour.
- **A story arc:** the problem → what TraceHound does → a real catch → how we know it works (blind test, seeded test) → what didn't work (one line; honesty is what sets it apart) → install in one file.
- **Every number comes from numbers.md**, worded the way §0 allows ("statistically tied with four of five commercial reviewers, ahead of one"). **Never say or show a competitor's name:** on screen they're "Reviewer A–E" with blurred logos. Credit the recall-seeded #7 catch to "the deterministic contract check, no model".
- The opening card says: logos are blurred, and TraceHound's reviewer runs only on NVIDIA Nemotron via Nebius Token Factory.
- Say "Nemotron 3.5 Super" and "Nebius Token Factory" clearly at least once each; the sponsors are judging.
- Iterate: draft, then read the voice-over against the shot list for timing (words per shot ÷ 2.5 = seconds) and fix any shot that runs long. Then do one pass purely for sound: does each line sound natural said aloud?

Then push both files (`git pull --rebase`, add only those two files, commit, push) and send them to Sunny.
