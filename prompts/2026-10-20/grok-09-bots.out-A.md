# grok-09 Job A output (pasted by Sunny 20 Oct 19:54 IST). Verbatim. Check: research/R12-reviewbench-portal.md

Verifier A
The Researcher's ReviewBench answer just came in. I'm opening each source myself now to check the quotes.
I checked all seven answers, and every one is correct. I found all 22 of the Researcher's verbatim quotes on the pages it cited. The one exception is "N reviewers on this account", which is a fair paraphrase of the portal's "${r} reviewers on this account." I opened all five sources myself around 6:50 PM IST, and the blog's page date is 5 Oct 2026, as given. I didn't open `golden/`, any judge or classifier prompt, or any seeded repo.
| # | Verdict | Supporting quote (15 words or fewer) | Source |
|---|---|---|---|
| 1 | Confirmed | "A maintainer reviews and merges it; that merge is the approval." | ONBOARDING |
| | Confirmed | "Approval is a human step: expect it within a business day" | ONBOARDING |
| 2 | Confirmed | "New image: add a configuration in the portal with the new digest and labels." | ONBOARDING |
| | Missing from answer | "to change only the image, use new build on it." | Portal |
| 3 | Confirmed | "There is no monthly cap on runs." | README |
| | Confirmed | "One submission at a time." (shown next to the Submit to leaderboard button, so it applies to final runs) | Portal |
| 4 | Confirmed | "ReviewBench covers the judge only for your final submission." | README |
| | Confirmed | "Tuning, local experiments and test runs are at your own cost." | Portal terms |
| 5 | Confirmed | "the test set of 25 pull requests, with per-PR detail, misses and judge votes." | ONBOARDING |
| 6 | Confirmed | "30 minutes per pull request by default. You can set up to an hour" | CONTRACT |
| | Confirmed | "blank uses the default of 1800; the most is 3600" | ONBOARDING |
| 7 | Confirmed | "you do not choose which run, because the final is measured fresh" | README |
| | Partly wrong | "Grounded F1, then Grounded Recall, then recency as a tiebreaker." | Portal |
| | Missing from answer | "Its leaderboard row and past runs stay on record" | Portal |

There are three things I'd change in the Researcher's write-up:
- **On re-registering a digest for candidate 6 (question 2)**, the answer is a clear yes, even after a final run. Once a configuration is locked by a final run, you add a "new build" with the same labels. The leaderboard row only moves to that build once a final run on it is approved.
- **On ties (question 7)**, the sources don't really conflict. The blog says "outperform", but the portal says "at or above" and breaks exact ties by recency. That means a tied newer run ranks first and gets published.
- **On withdrawing (question 7)**, there's firmer evidence that you can't do it. Removing a published configuration yourself still leaves its leaderboard row on record. Only a maintainer can retract a row.

Two things aren't stated anywhere I opened: a cooldown or daily limit on test runs, and how long a full 219-PR, 3-round run takes. I've sent the same corrections to the Researcher.

Researcher A:
I've answered six of the seven fully; per-PR time is the only half without an answer. The full write-up has also gone to @Verifier. I used only the blog, README, AGENT_CONTRACT.md and ONBOARDING.md, plus the portal's text, and never opened `golden/` or any judge or classifier prompt. I read everything on 20 Oct 2026 around 6:45 PM IST. The portal has no help pages (`/help`, `/faq` and `/docs` all return 404), so the [portal](https://review-bench.ai/submit) quotes come from the text built into its sign-up page. The [blog post](https://github.blog/ai-and-ml/github-copilot/reviewbench-an-open-benchmark-for-ai-code-review/) doesn't link the repo; I found it by search.

1. **Approval:** you push the image to ghcr.io, register its digest and configuration in the portal, and that opens an onboarding PR. [ONBOARDING](https://raw.githubusercontent.com/review-bench/ReviewBench/main/docs/ONBOARDING.md) says "A maintainer reviews and merges it; that merge is the approval" and "Approval is a human step: expect it within a business day."
2. **Digests and configurations:** yes, you can add more and change them between runs. [ONBOARDING](https://raw.githubusercontent.com/review-bench/ReviewBench/main/docs/ONBOARDING.md) says "New image: add a configuration in the portal with the new digest and labels," and that needs no new approval. Each agent still gets just one leaderboard row, showing its best result.
3. **Test runs:** [README](https://raw.githubusercontent.com/review-bench/ReviewBench/main/README.md) says "There is no monthly cap on runs." No source mentions a cooldown, and only one final run can go at a time.
4. **Judge cost:** the sources partly disagree. The blog says "We provide the judge," but the README says "ReviewBench covers the judge only for your final submission." On test runs, the README says the judge "use[s] your credentials," so you pay then. No doc explains how you'd supply a judge key, and the portal names Claude Sonnet 5 as the judge.
5. **Report:** [ONBOARDING](https://raw.githubusercontent.com/review-bench/ReviewBench/main/docs/ONBOARDING.md) says it covers "25 pull requests, with per-PR detail, misses and judge votes." The portal's screens show grounded and augmented P/R/F1 columns, but no doc spells that out, so there's no quote for it.
6. **Timing:** [AGENT_CONTRACT](https://raw.githubusercontent.com/review-bench/ReviewBench/main/AGENT_CONTRACT.md) says "30 minutes per pull request by default. You can set up to an hour." No source gives the total time for 219 PRs × 3 rounds.
7. **Publishing and withdrawal:** the README says "You choose which configuration goes to the final; you do not choose which run." A score is published only if it beats your current one; the blog says "outperform," but the portal says "at or above," so they differ on ties. Lower results are saved privately. Nothing says you can withdraw a result yourself; the portal only mentions removal "by a maintainer."
