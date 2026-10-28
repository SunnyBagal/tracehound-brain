# ui-11c: three fixes before #65 merges

Tool: **the running UI session** (Claude Code, account B, `~/Projects/TraceHound-ui`, branch `ui/v2`, PR #65). Paste below the line.

---

The chat checked #65 at `690c6b4` merged onto main (clean merge; viewer tests 212/212, typecheck green; `next build` couldn't fetch Google Fonts in the chat's sandbox, so rely on CI and Vercel for the build). Three things before it merges, one commit:

1. **No competitor logo on the landing page.** `components/landing/Hero.tsx` `MARKS` has a **Cursor** mark (`brands/cursor.svg`). Cursor is one of the five benchmarked reviewers. The rule since the ui-11 brief: no competitor names or logos on the landing page, and Cursor is never named in the Devpost text or video. Remove the Cursor mark. If the white pack needs the same count, use a generic mark (an unlabelled shape or a git/PR glyph). Keep the owner's other marks unless he says otherwise.
2. **Delete the unused competitor logos** from `viewer/public/icons/brands/`: `coderabbit.svg`, `augment.svg`, `greptile.svg`, `githubcopilot.svg`, and `cursor.svg` after step 1. They aren't referenced, but `public/` ships them on the live site. Check `rg -n "brands/(coderabbit|augment|greptile|githubcopilot|cursor)" viewer` prints nothing, and update `CREDITS.md`/icon source lists if they list them.
3. **Turn the seeded lane on:** `viewer/lib/repos.ts` `PR_LANE_FLAGS.recallSeeded: true` (scoring finished 28 Oct; the PRs are public results now). Update any test that asserts it's off.

Gates (viewer test, typecheck, build, as separate commands), push, report the head sha. Don't merge.
