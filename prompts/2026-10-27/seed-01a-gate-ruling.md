# seed-01a gate ruling (chat, 27 Oct 23:45): continue, with a 3-PR gate

Paste below the line into the **seed-01a session** (Claude Code, account A, `~/Projects/tracehound-seeds`).

---

Good stop. The chat read the Action at `0aeef06` (you can't): the log line you got is evidence the model **was** called and answered.

- If the key, route or model were wrong, every reviewer chunk would fail, and `pipeline.ts` (l. 134) throws `StageError("reviewers")`, which logs **"reviewers failed or returned invalid output; nothing posted."**
- You got **"reviewer returned no findings; nothing posted."** (`action.ts` l. 293). That path is only reached when the reviewer stage returned valid JSON with zero findings left after pool, verify and contracts. The one exception is a PR whose diff produced **zero reviewer chunks**.
- Your run log already shows `model: nvidia/Nemotron-3.5-Super-VL` at `0aeef06`. The 20 Oct failure this gate guards against was an *unset* model silently falling back to Nano, which is not the case here.

So s15 on `recall-seeded#1` is a **result: no review posted (counts as a miss, run green)**. Don't rerun it.

Continue Step 2 with a **3-PR gate**, then the rest:
1. For `recall-seeded#1`, record `gh pr diff 1 -R SunnyBagal/recall-seeded --name-only` and the diff's added and removed line counts. Counts only in the report; this rules out the zero-chunk case.
2. Resume the script (your `opened-prs.tsv` keeps the pacing) and open **the next 2 PRs only**. Wait for both runs.
3. **Gate:** if at least one of the 3 runs posted a review whose header names `Nemotron-3.5-Super-VL`, the live config is confirmed: open the remaining PRs. If all 3 are green with no review **and** each reviewer step took under 30 s, STOP and report the three logs' TraceHound lines and step durations. A red run: rerun once, as in Step 3.
4. Step 3 onwards as written. In the results, list every "green, no review posted" PR with its reviewer-step duration.

Yes, write the stop report to the brain inbox now as `2026-10-27-sunny-seed-01a-stop.md`, counts and links only, then continue.
