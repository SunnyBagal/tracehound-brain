# bunny-04 report: Devpost drafts

**PR:** #58 (draft, not merged): https://github.com/SunnyBagal/TraceHound/pull/58
**Head:** `3d5a62de9c6ca37b70907e2348298aef646387da`, branch `docs/devpost-drafts` from `origin/main` @ `8712273`

## Commits (all in `docs/devpost/`; docs only)
- `aaedf62` docs(devpost): write-up draft with placeholders for the 26 Oct numbers
- `c74b038` docs(devpost): what changed since 26 August, from git and the decisions log
- `2132263` docs(devpost): one-page summary of FEEDBACK.md for the Nebius/NVIDIA feedback field
- `9d149f3` docs(devpost): 2:50 video script, shot by shot, no competitor names
- `3d5a62d` docs(devpost): submission checklist with status and owner per item

## Tests
- Step 0: the first `pnpm test` had 1 failure, a 5,000 ms timeout in `test/contracts-findings.test.ts` ("control: HTTP route path changed, client fetch updated"). The immediate full rerun passed. I took it as a load flake and went on; flagging it because Step 0 says stop on any failure.
- Before every commit: GIT_* env check printed nothing, and `pnpm test` and `pnpm typecheck` were green. Counts: action 237 passed; analyzer 880 passed, 7 skipped; viewer 188 passed.

## [[SUNNY: …]] questions left
1. writeup.md, Inspiration: the personal reason (2–3 sentences).
2. writeup.md / checklist #11: decision 066's claim names three reviewers, and the Devpost text must name none. I drafted it with the names replaced by descriptions ("the two reviewers just above and below it", "the lowest-scoring reviewer"). Approve or reword.
3. writeup.md: whether to name the AI coding assistants that built it (trademark rule).
4. writeup.md: anything else for "What's next".
5. writeup.md / checklist #9: YouTube URL.
6. what-changed: what existed before 26 Aug (FixFlow?), and where; whether any code or design was carried into the 30 Sep commits; plus a one-sentence version for the Devpost field.
7. what-changed: whether the 30 Sep work (76 commits, decisions 001–031, 02:34–23:37 IST) was written that day or brought in from earlier work. Note: `docs/reviewer-plan.md` says the repo "predates the hackathon", but its git history starts on 30 Sep.
8. checklist #6: I read "README pin" as the Action reference in the README install block. If it meant something else, say so.
9. checklist #7: is "browse recorded reviews + dashboard" enough for "demo works without a judge's own key", or should judges be able to trigger a review?
10. checklist #7: confirm Vercel stays up until 15 Dec.
11. video-script / checklist #10: may the NVIDIA and Nebius marks and GitHub's UI appear on screen?
12. video-script shot 6: "plan and freeze tag committed first" holds for the v2 run only if 069's plan and review-freeze-2 came before it. Check on 26 Oct.

## Found while checking (not changed; outside allowed paths)
- The repo's About description still has the pre-pivot framing ("Persistent repository intelligence … guides sandboxed agents toward tested fixes"). Topics are empty. Licence: `gh repo view --json licenseInfo` → MIT, so GitHub has detected it. I didn't look at the sidebar visually.
- #54 is stale after 068. Its install block uses `@main` with `model: nvidia/nemotron-3-super-120b-a12b # The benchmarked model`, and "Built with" says "Nemotron 3.5 Super is being evaluated (decision 067)". The checklist lists both for the fill job.
- `/benchmark` names the other reviewers in its tables, and the graph shows technology logos. The video script keeps both off screen.
- Dashboard `/`, `/reviews` and `/benchmark` returned HTTP 200 on 23 Oct. `/reviews` reads GitHub's public API without a token, so it is subject to the unauthenticated rate limit per viewer.

## Couldn't do
- Decision 069 isn't in the repo yet, so the v2 Cal.com run is referenced only through placeholders.
- `pnpm spend` in this worktree reads an empty ledger, so the drafts take spend figures only from FEEDBACK.md and the docs.
