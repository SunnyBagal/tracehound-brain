# TraceHound handoff, 29 Oct 2026, 22:20 IST

Paste into a new Claude chat with: "Continue TraceHound from this handoff." **Deadline 30 Oct 22:30 IST.** Submit tonight or tomorrow morning.

## Role
Planning chat: short, blunt, honest. Check facts against `submission/numbers.md` (wording rules §0: never "beats/best/outperforms"; "ahead/behind" only where the interval excludes 0; "statistically tied" otherwise; no competitor names or logos in the Devpost text or video). Sunny merges with "Create a merge commit": say "commit".

## State
- **TraceHound main `8fbac41`** (#69 merged): Q6/Q7 in `docs/devpost/what-changed-since-26-aug.md`; one flag `BLUR_THIRD_PARTY_MARKS` (`viewer/lib/marks.ts`) blurs third-party logos (hero, benchmark, graph tech icons) and shows the five reviewers as **Reviewer A–E** on /benchmark (`viewer/lib/benchmark-display.ts`; results.md keeps the names). Limits under Cost with the wolves cut-out on the footer line. Live: https://tracehound-tau.vercel.app
- Product frozen: pipeline `review-freeze-2` (`0aeef06`), Action `review-freeze-2.1` (`86fd5d7`, install fix only).
- Hosts on Discord: logos must be blurred (done); names undecided → A–E.
- Devpost draft: "TraceHound: Evidence-Backed PR Reviews on NVIDIA Nemotron". Project overview and Additional info done; **Project details incomplete** (About, Built with, video).

## Left
1. Sunny records the video (`submission/video-script-v3.md`, ≤ 2:50, never say a competitor name), uploads to YouTube **Public**.
2. Devpost: About = `submission/devpost-about.md` (put the YouTube URL in; Built with without `bun`); video URL field; demo URL `https://tracehound-tau.vercel.app`; form answers = `submission/devpost-form-answers.md`; optional thumbnail (3:2, no third-party logos). Preview, then **Submit**.
3. Chat: one small PR replacing `docs/devpost/writeup.md` with the final About + YouTube URL (it still has `[[SUNNY]]` slots for Q1 and the URL). Push access works from the chat via `add_repo` push.
4. After submit: nothing paid; balance $38.78 before the seeded runs (~$3 used).
