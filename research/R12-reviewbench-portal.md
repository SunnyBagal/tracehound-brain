# R12 · How the ReviewBench portal works (grok-09 Job A, 20 Oct 2026)

**Source:** Grok Researcher, then Verifier, on 20 Oct around 18:45–18:50 IST. Raw text: the chat transcript of 20 Oct 19:54, filed in `prompts/2026-10-20/grok-09-bots.out-A.md`.

**Kill test (P003):** the chat opened README.md and docs/ONBOARDING.md itself (raw.githubusercontent.com, review-bench/ReviewBench main). Answers 1, 2, 3 and 7 match. Pass.

| Question | Answer | Effect on us |
|---|---|---|
| Approval | The portal opens an onboarding PR; a maintainer merges it; "within a business day" | Register **now**. The approval wait runs alongside the smoke runs |
| New digest later | "add a configuration in the portal with the new digest and labels". **No re-approval** | Candidate 6 can use a later image. No blocker |
| Test-run cap | "no monthly cap on runs". No cooldown stated. One final run at a time | Six test runs are fine |
| **Judge cost** | Final: "at our cost". **Test runs: self-funded ("your credentials and compute")**. How the key is supplied isn't documented; the judge is Claude Sonnet 5 | **New cost:** Anthropic API credit for 6 test runs. R7 estimated ~$3 per 25-PR set → **~$18**. To be confirmed at registration |
| Report | 25 PRs with per-PR detail, misses and judge votes | Note: per-PR misses tell us *which* expected issues we missed. That's test-set feedback, allowed by P008, but **we tune nothing on it** |
| Time | 30 min per PR by default, up to 60 (`timeout` up to 3600 s). Total time for 219 × 3: not stated | Our wall limit is 25 min. Leave the default |
| Publishing | We choose the config, not the run; the final is "measured fresh". A row updates only on a score "at or above" the current one; ties go to the newer run; **no self-withdrawal** (only a maintainer can retract) | The first final is permanent. That's why P008 allows one final only |
