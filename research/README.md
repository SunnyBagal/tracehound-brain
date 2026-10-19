# Research

External research, mostly from Grok, checked by the chat before filing. A report is filed only after its citations are spot-checked: 5 random URLs opened. One invented or misquoted source means the report is dropped, as with the Grok kill test ([P003](../decisions/P003-grok-kill-test.md)).

| ID | Question | Feeds | Status |
|---|---|---|---|
| R1 | Filtering LLM review findings without losing recall | A possible precision round on 19–20 Oct, before the freeze | **Filed** ([R1](R1-verifier-methods.md)); 5/5 citations check out |
| R2 | AI code review market, benchmarks, developer complaints, judge self-preference | README, Devpost write-up, results-doc framing, 060 limits | **Filed** ([R2](R2-market-benchmarks.md)); 5/5, two small overreaches noted |
| R3 | Hackathon rules fine print, other submissions, winning demos | Demo repo and video plan | **Filed** ([R3](R3-hackathon-demo.md)); 5/5 |
| R4 | How the top reviewers get precision (grok-05) | 064 voting; v2 gates | **Filed** ([R4](R4-precision-techniques.md)); 3 links spot-checked |
| R5 | Hackathon rules and Nebius/NVIDIA levers (grok-06) | Coding track framing; required sections | **Filed** ([R5](R5-hackathon-and-nebius-levers.md)) |
| R6 | Independent audit of the results (cc-01, #41 draft) | Results wording fixes; public claims | **Filed** ([R6](R6-cc01-audit.md)); numbers correct, 5 wording overclaims |
| R7 | ReviewBench feasibility (cc-02, #42 draft) | Optional extra held-out test | **Filed** ([R7](R7-cc02-reviewbench.md)); needs Anthropic API credit for the judge |
| R8 | grok-07: which competitors run free on public repos; how the top tools learn; cross-file claims | Seeded test (cc-04), S8 design | **Filed** ([R8](R8-competitors-learning.md)). The links were lost in the paste, so the kill test couldn't run; the chat checked 6 decision-relevant claims on primary pages: 5 true, 1 date wrong, Bugbot pricing conflicts |

| G1 | Audit of results docs 058–060 | Integrity of the published numbers | **Passed** 2026-10-17: no findings; 6/6 cited lines checked |
| G2 | Video script, first draft (placeholders for results) | Video, 27–29 Oct | Filed below; needs revising for the dashboard and fix button |

Prompts: `grok-02-research.md`, `grok-03-audit-and-script.md` (owner's Downloads). Blindness: Grok is told never to open `ai-code-review-evaluations` or Cal.com PR material.


## G2 · Video script draft (Grok, 2026-10-17)

The draft runs about 2:40. It names Token Factory and Nemotron at 0:18 and at the close, and puts results in placeholders (`[TEST-SET F]`, `[RANK OF 6]`, `[DEMO: N OF 4 SEEDED BUGS CAUGHT]`, `[CONTROL PR COMMENTS]`).
- **Shots:** problem (noise) → Action on Nemotron via Token Factory → two-reviewer + verifier diagram → demo PR with a seeded bug caught; control PR quiet → verifier keeps 69% of real findings / a third of false ones → benchmark table with placeholders → about 3¢ per PR, open source → close.
- **To revise:** add the dashboard (bug cards, graph pin) and a "Commit suggestion" click. Lead with a demo-first opening.
