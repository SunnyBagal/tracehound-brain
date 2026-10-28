# codex-18b: restore the blind-test card in the video (shot 6) + one known-limit line

Tool: **ChatGPT Codex, the same task as codex-18** (`~/Projects/TraceHound-codex`). Paste everything below the line as a follow-up.

---

The chat's claims pass on #54 (`6c6fb95`) and #58 (`32833ab`) found one real problem, caused by an ambiguous line in codex-18 (the chat's fault): **shot 6 of `video-script.md` lost the blind Cal.com headline**. The video now has no benchmark number at all. Fix it, plus three small items. Docs only, $0.

## On `docs/devpost-drafts` (#58)
1. **`video-script.md` shot 6**: replace the whole row with the original row from `8184e54` (`git show 8184e54:docs/devpost/video-script.md | grep "^| 6 "`), changing **only** `"Seeded demo: [[TBD-SEEDED]]."` to `"Seeded test (pre-registered, 30 PRs): 9 of 22 seeded defects caught, 0 of 8 clean controls flagged."`. Keep its voice-over exactly as in `8184e54`. Shot 2 already narrates the seeded catches and the contract-check split, so the full seeded sentence isn't repeated.
2. **`video-script.md` shot 7**: "`docs/bench/results.md` §6" → "§7" (the levers table moved in #66).
3. **`video-script.md` "Before shooting"**: the RESULTS.md details bullet stays; the top paragraph's "at `review-freeze-2` (`0aeef06`)" gets ", the Action at `review-freeze-2.1` (`86fd5d7`)" appended.
4. **`writeup.md` "What's next"**, a new first bullet (no names): "The reviewer's retry doesn't tell a reply cut off at the token limit from malformed JSON; an automated code review found this. Fixes are on main behind off-by-default options, not measured (`docs/post-freeze-fixes.md`)."
5. Check: shot timings still sum to ≤ 2:50; `grep -c "57.1" video-script.md` ≥ 1; `grep -c TBD-SEEDED` = 0; the names grep is still empty.

## On `docs/judges-readme` (#54)
6. **README "What's next"**, the same bullet as item 4, with "an automated code review (Devin)" allowed here (the README may name tools).

Push both. Report: head shas, the new shot 6 row, the two new bullets, the checks. Inbox: append to `2026-10-28-codex-18.md` (pull --rebase, commit, push).
