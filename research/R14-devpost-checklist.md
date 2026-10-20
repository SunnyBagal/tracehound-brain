# R14 · Devpost submission checklist (grok-10 Job E, 20 Oct 2026)

**Source:** Grok Researcher and Verifier, about 20:45 IST. The Verifier confirmed all 49 quotes, from the [rules](https://nebiusglobalaihackathon.devpost.com/rules), the [overview](https://nebiusglobalaihackathon.devpost.com/), the [FAQ](https://nebiusglobalaihackathon.devpost.com/details/faqs) and the [organiser update](https://nebiusglobalaihackathon.devpost.com/updates/46205-how-to-build-a-winning-project). Chat check: the LICENSE exists on TraceHound main (MIT; `package.json` says MIT).

| Requirement | TraceHound status | Action |
|---|---|---|
| Deadline 30 Oct 2026, 22:30 IST | target 29 Oct | — |
| YouTube video under 3 min, public, shows it working; judges needn't watch past 3:00 | not made | 27–28 Oct |
| **No third-party trademarks in the video without permission** | risk: a table naming Augment, Cursor, Greptile, Copilot or CodeRabbit | **The video shows no competitor names or logos.** Show TraceHound's own numbers and "comparison in the repo's results doc". Names stay in the repo docs (factual reporting; not legal advice) |
| Public repo with a license file visible in the About section | MIT LICENSE exists | Check the GitHub "About" sidebar shows the license |
| README with setup, highlighting Nemotron and Token Factory | bunny-02 in progress | judges' README |
| Live runtime calls to Token Factory (or Nebius AI Cloud); NVIDIA open model | yes: Nemotron 3 Super via Token Factory | say it in the README and video |
| Track, description, feedback on Nebius and NVIDIA, in English | FEEDBACK.md exists | Devpost write-up 26 Oct |
| **Working demo, free, no restriction until judging ends** | dashboard tracehound-tau.vercel.app + demo-repo PRs (viewable with no key) | The demo must **not** require a judge's own Nebius key. Point to live PR reviews and the dashboard |
| Existing project "significantly updated" after 26 Aug, with an explanation | git history starts 30 Sep; decisions 001–031 date from 30 Sep | Short "what changed since 26 Aug" paragraph (open question in NOW) |
| Third-party integrations "must be authorized" | CodeRabbit §4.2(iv), Greptile §1.4(a) (R13) | Sunny's call; Greptile consent email |
| Judging: Stage One pass/fail, then 4 equal criteria scored 1–5 (Technological Implementation, which includes how well NVIDIA models are used; Design; Potential Impact; Quality of the Idea). Ties broken in that order | — | The video and README should lead on technical implementation + Nemotron use |
