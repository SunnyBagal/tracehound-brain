# ui-11c: before #65 merges (revised 29 Oct 02:45: the owner keeps the logos)

Tool: **the running UI session** (Claude Code, account B, `~/Projects/TraceHound-ui`, branch `ui/v2`, PR #65). Paste below the line.

---

The chat checked #65 at `690c6b4` merged onto main (clean merge; viewer tests 212/212, typecheck green; `next build` couldn't fetch Google Fonts in the chat's sandbox, so rely on CI and Vercel for the build). The owner keeps the hero's marks (the agents whose pull requests TraceHound reviews). Two small things before it merges, one commit:

1. **Trademark line.** Under the hero (small, muted, one line): "Logos are trademarks of their owners and are used only to name the coding agents whose pull requests TraceHound reviews. No affiliation or endorsement." Also add the same sentence to `CREDITS.md` with the list of marks shown.
2. **Turn the seeded lane on:** `viewer/lib/repos.ts` `PR_LANE_FLAGS.recallSeeded: true` (scoring finished 28 Oct; the PRs are public results now). Update any test that asserts it's off.

Gates (viewer test, typecheck, build, as separate commands), push, report the head sha. Don't merge.
