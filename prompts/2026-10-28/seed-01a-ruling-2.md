# seed-01a ruling 2 (chat, 28 Oct 01:45): Recall now, Linea after a re-pinned Action; Devin Review removed by Sunny

Paste below the line into the **seed-01a session**. **Sunny first** removes the Devin app's access to both mirrors (steps in the chat reply); the agent checks that before resuming.

---

Good stop again; both blockers are real.

**Devin Review.** Sunny is removing the Devin GitHub app's access to `linea-seeded` and `recall-seeded`. Before you resume, check: open no new PR until Sunny confirms in this session that access is removed. Leave the 3 existing Devin comments untouched (no deleting). The scorer ignores them. Record it as a deviation: "a third-party reviewer app (Devin Review) was installed account-wide and auto-reviewed the first 3 seeded PRs; removed before the rest opened; its comments are not scored."

**Linea: a fourth option.** The install failure is a real bug in TraceHound's Action, and judges hit it too. It's being fixed as **`review-freeze-2.1`**: `0aeef06` plus a change to the Action's install step only, with the review code byte-identical. So:
1. **Now:** resume the script for **Recall PRs only**, skipping Linea branches as they come up in the order (disclosed deviation: "Linea seeds opened after the Action fix, same order otherwise"). Then Step 3 (wait / one rerun of red runs) for Recall.
2. **Later (the chat will send the new pin, likely 28 Oct morning):** update only the `uses:` line of `linea-seeded`'s workflow to the 2.1 commit (one commit; `recall-seeded` stays at `0aeef06`). Then re-trigger `linea-seeded#1` **without closing it**: `gh pr ready 1 -R SunnyBagal/linea-seeded --undo`, then `gh pr ready 1 -R SunnyBagal/linea-seeded` (a `ready_for_review` run uses the base branch's current workflow). Then open the remaining 14 Linea PRs with the 90 s gap.
3. Score as written once all 30 have finished. Results report Recall and Linea separately, and say which Action commit each ran on. The review code is identical on both, which `git diff` in decision 072 shows.

Write this stop as `inbox/2026-10-28-sunny-seed-01b-stop.md` (counts and links only), then wait for Sunny's "Devin access removed" before opening anything.
