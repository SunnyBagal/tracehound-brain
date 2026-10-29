# TraceHound handoff, 30 Oct 2026, 00:15 IST — submission day

Paste into a new Claude chat with: "Continue TraceHound from this handoff." **Devpost deadline: 30 Oct 22:30 IST.**

## Role
Planning chat: short, blunt, honest. Every number from `submission/numbers.md` (brain); wording rules §0: never "beats/best/outperforms"; "ahead/behind" only where the 95% interval excludes 0, otherwise "statistically tied"; no competitor names or logos in the Devpost text or video (Cursor not even as a tool). Merges: "Create a merge commit" → say "commit". The chat has push access via `add_repo` (TraceHound, Recall, tracehound-brain).

## State (all merged, live at https://tracehound-tau.vercel.app)
- TraceHound main `b498647`. Product frozen: pipeline `review-freeze-2` (`0aeef06`), Action `review-freeze-2.1` (`86fd5d7`, install fix only, decision 072).
- Site: one flag `BLUR_THIRD_PARTY_MARKS` (`viewer/lib/marks.ts`) blurs third-party logos and shows the five benchmarked reviewers as **Reviewer A–E** (`viewer/lib/benchmark-display.ts`; results.md keeps names). Set false after judging to restore. Limits under Cost with the wolves cut-out.
- Recall main `256513a` (orphan `Usecontent.tsx` deleted; `SidebarToggle.tsx` gone since `62eaca1`). Snapshot regenerated: 0 warnings, 4 components renamed fresh by Nemotron 3 Super (e.g. "Recall API Service"). Local ledger $40.44 of the $45 cap.
- Seeded test done: 9/22 caught, 0/8 controls flagged; 3 of 4 cross-file catches are the deterministic contract check (say so).
- Hosts (Discord): blur logos (done); names undecided → A–E.

## Files (brain `submission/`)
- `devpost-about.md` — About section (replace `YOUR_YOUTUBE_URL`; Built with: drop `bun`).
- `devpost-form-answers.md` — every form field. Plain-text versions with "1)" numbering were given in chat for: models, rating (8), compare, Nebius capabilities, improvements, Nemotron next, AI tools. Suggested ratings 8 / 9 / 9 with reasons. Demo URL = https://tracehound-tau.vercel.app. "New" project; Tavily: No.
- `video-script-v3.md` — 2:50: card → home → recall-seeded #4 (model catch) → #7 (contract check, "no model") → full /benchmark → /graph (Recall, pick PR #4 in the PR lane, sidebar evidence) → install YAML. Never say a competitor name. Face cam optional.
- AI tools answer: Claude Code (+relay), Claude chat, Codex, Devin, Grok/Gemini research, TraceHound dogfood; no Cursor/Bugbot named.

## Gallery (3:2, from the live site; captions in chat 29 Oct 23:0x)
1. Graph with PR #4 selected + Findings sidebar — **done** (`01-graph-pr4-3x2.jpg`, padded to 2400×1600); also the thumbnail.
2. recall-seeded #4 review (header with `nvidia/Nemotron-3.5-Super-VL` + line comment)
3. recall-seeded #7 "Files you didn't edit that depend on this change"
4. /benchmark top (tiles + blind-test figure)
5. /benchmark seeded or all-50 table
6. /reviews bug cards
7. Homepage hero
To make a screenshot 3:2: chat pads with the page colour (14,14,19) and exports JPEG ≤ 5 MB, or ⌘⇧4 at ~1500×1000.

## Left
1. Sunny: record → YouTube **Public** → URL.
2. Devpost: Project details (About, Built with, video URL), Media (gallery + video), form answers, thumbnail → Preview → **Submit**.
3. Chat: one PR replacing `docs/devpost/writeup.md` with the final About + YouTube URL (it still has `[[SUNNY]]` slots for Q1 and the URL).
4. After judging (not now): flip `BLUR_THIRD_PARTY_MARKS` back if wanted.
