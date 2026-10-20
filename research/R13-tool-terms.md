# R13 · Benchmark clauses in the seeded-test tools' terms (grok-10 Job D, 20 Oct 2026)

**Source:** Grok Researcher and Verifier, around 20:00–20:10 IST. All 19 quotes confirmed by the Verifier. **Chat spot-check: none yet.** These are legal texts, so Sunny decides; this isn't legal advice.

| Tool | Benchmark clause | Automated or bulk use | What it means for the 25 Oct seeded test |
|---|---|---|---|
| **Greptile** (ToS, updated 17 Oct 2026) | No benchmark clause. **§1.4(a)** bars making "any results of the Services available to any third party" without written OK | §1.4(c): nothing that threatens integrity, performance or availability | Publishing Greptile's seeded-test output without consent arguably breaches §1.4(a), which is broad. **Ask Greptile for written OK** |
| **Cursor Bugbot** (ToS, updated 3 Sep 2026; AUP, updated 11 Aug 2026) | **§1.5(vii):** no sharing of benchmark results "unless you include all necessary information for others to replicate the tests" | AUP: no access "through automated or non-human means… bot, script"; no avoiding fees | **Allowed if we publish a full replication kit** (mirrors, the key, the scorer, raw reviews). Our script opens *GitHub* PRs; Bugbot is triggered by GitHub, not by our script. Low risk, but noted |
| **CodeRabbit** (ToS, updated 20 Apr 2026; MSA, updated 18 Jun 2026) | **§4.2(iv):** no disclosure of benchmarking results without prior written consent | §4.2(vi): no automated data extraction | **Sunny decided on 19 Oct to keep CodeRabbit.** Not re-argued |

## What to do
- **Email Greptile now** for written permission to publish its result on our pre-registered seeded test. It's cheap and they may say yes.
- **Publish the replication kit with the seeded-test results**, as Cursor's §1.5(vii) requires: after scoring, make `tracehound-seeds` and the mirrors public, with the scorer and raw reviews.
- **If Greptile doesn't answer by 26 Oct**, publish TraceHound's seeded numbers and say Greptile was run but its result is withheld pending consent. Or publish anyway: Sunny's call, as with CodeRabbit.
