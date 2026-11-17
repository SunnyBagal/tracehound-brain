# TraceHound product decision · 17 Nov 2026

Raw report, unreviewed, uncommitted. Read-only session, $0, no model call. Builds on the 16 Nov audit. Tags as
there: **VERIFIED** (checked today), **DOCUMENTED** (a repo file or web page says so), **INFERRED** (my reasoning;
every threshold is a judgement), **UNVERIFIED**.

## Summary

1. No held-out result or competitor comparison exists; nothing below assumes one. VERIFIED (STATE, 076r)
2. The band is the gap in bugs caught of 20 against the better of CodeRabbit and Greptile: +4 or more is a paid pilot, −3 to +3 the narrow claim, −4 or −5 open source only, −6 or worse stop. INFERRED
3. Dev (all 31 bugs, and the 21 unexposed) can lower the band by one and never raises it. INFERRED
4. The frozen pipeline costs about $0.066 per review. Demand is the constraint: 5 stars, 0 users. DOCUMENTED cost, VERIFIED stars
5. Tiers: free with your own key, $0.50 per hosted review, $12 per developer per month; paid tiers only in band A. INFERRED
6. The model question needs one new arm, a Nemotron agent on the same skill, beside the registered no-lookup arm, confirmed once on the opened held-out set. INFERRED
7. Largest expected losses: few bugs are cross-file, competitors post about three times as many comments, and per-finding verification has removed true positives before. DOCUMENTED counts, INFERRED ranking
8. Company work stops on 15 Dec without one outside user and 25 stars, and on 31 Dec without a paying user. INFERRED
9. Out of date in the audit: the deletion-only gap (S1) and the no-lookup mode are built on the side branch; 0 of 31 dev fixes predate July 2026, which weakens S3. DOCUMENTED (16 Nov side-track report, unreviewed)
10. Not verifiable today: payment fees, hosting terms, every side-track cost. UNVERIFIED

## Cannot be decided before the held-out result

- Which band applies, so whether pricing or hosted work starts.
- Whether the narrow cross-file claim gets its test (band B only).
- Whether the model test's held-out step runs: it needs the firewall lift and leftover money.
- Whether A0's dev catches hold on unseen code.
- Whether 50 stars is reachable: nothing is published yet.

## 1. Decision rule

Headline measure (protocol rev 13 §4): distinct labelled bugs caught of 20, human-confirmed, A0 against CodeRabbit
and against Greptile. DOCUMENTED

`D_H` = A0's catches minus the competitor's on each registered pair's eligible cases; the worse pair counts. `D_31`
and `D_21` are the same on all 31 dev bugs and on the 21 unexposed. INFERRED

| Band | `D_H` | Outcome | Source of the number | Tag |
|---|---|---|---|---|
| A | +4 or more | Paid product, as a pilot until §5's user test passes | The project's noise floor (audit §7) | INFERRED |
| B | −3 to +3 | Narrow to the cross-file TypeScript monorepo claim if task 8 passes; otherwise C | The protocol's GO margin | INFERRED |
| C | −4 or −5 | Open source only: no pricing, no hosted service, no spend | Below the smallest gap that can be significant | INFERRED |
| D | −6 or worse | Stop reviewer work; publish result and method | Protocol §6 and the RETHINK line | INFERRED |

Modifiers, in order (all INFERRED):

1. **Dev demotion.** One band down if `D_31` ≤ −6 and `D_21` ≤ −4 (6 of 31, scaled). Both, because the 10 exposed
   cases may favour the competitors. Dev never promotes: it has had at least 10 looks.
2. **Floor.** A0 catches 3 or fewer of 20 (Wilson upper bound 36%): band C at best.
3. **Comments.** A0's mean comments per PR above twice the competitor's: one band down.
4. **Coverage.** Under 15 of 20 reviewed by both tools, a pair gives counts only (G10). If both pairs fail, band C
   holds until a fresh set exists.

Limit: at n = 20, power is near 0.4 for a 30-point gap (protocol §6, DOCUMENTED). Bands B and C differ by less than
the set resolves. INFERRED

Evidence that would change a threshold: a power enumeration for a larger set; a second hand-checker moving a count by
2 or more; 5 or more exposed held-out bugs (then the unexposed row replaces `D_H`). INFERRED

## 2. Pricing

Public prices, read 17 Nov 2026 (DOCUMENTED):

| Tool | Price | Source |
|---|---|---|
| CodeRabbit | Free; Essentials $24, Team $48, Advanced $72 per developer per month billed annually ($30, $60, $90 monthly); 5, 8, 10 PR reviews per developer per hour | [plans](https://docs.coderabbit.ai/management/plans) |
| Greptile | Starter free: 1 developer, 50 credits a month. Pro $1 per credit, no seat fee; a review is 1, 3 or 10 credits | [pricing](https://www.greptile.com/pricing) |
| Cursor Bugbot | Usage-based since May 2026, replacing $40 per seat; "the average Bugbot run costs $1.00-$1.50" | [post](https://cursor.com/blog/may-2026-bugbot-changes) |
| Graphite | Team $40 per user per month billed annually, unlimited AI reviews; Hobby free, limited | [pricing](https://graphite.com/pricing) |

Graphite's Starter price and Bugbot's current rate card did not load. UNVERIFIED

Cost per review:

| Reviewer | Cost | Basis | Tag |
|---|---|---|---|
| A0, Nemotron pipeline | $0.066 | R1: $2.80987 less $0.54030 of failed-request charges, over 40 TypeScript cases = $0.0567, times the 1.17 ledger factor (076r). Fix stage never measured | DOCUMENTED inputs, INFERRED sum |
| Haiku 5.5, pipeline | $0.008 | $0.30288 over 40 cases | DOCUMENTED |
| Sonnet 5.5, pipeline | $0.170 | $1.36342 over 8 cases | DOCUMENTED |
| Sonnet agent with the skill | $0.12 | 076p's median list price; run records gone (audit S5) | UNVERIFIED |

Assumptions, none measured (there are no users):

- 30 reviews per developer per month; fair-use cap 60. UNVERIFIED
- Payment fee 2.9% plus $0.30 per charge. UNVERIFIED
- Fixed cost $20 a month: one Vercel Pro seat ([pricing](https://vercel.com/pricing), DOCUMENTED). Whether the free
  plan allows commercial use is UNVERIFIED. Sunny's time is priced at $0.

Tiers (prices are judgements, INFERRED):

| Tier | Price | Includes | Margin per review | Payers to cover $20 |
|---|---|---|---|---|
| Open | $0 | Action and skill on the user's own model key; contracts check | $0 | None |
| Per review | $0.50, as a $20 pack of 40 | Hosted, TraceHound's key, hard cap per review | $0.50 − $0.066 − $0.022 fee = **$0.41** ($0.36 at the Sonnet agent's cost) | 2 customers, one pack a month each |
| Team | $12 per developer per month | The same, up to 60 reviews per developer | At 30 reviews: $12 − $1.98 − $0.65 = $9.37 a seat, **$0.31** a review; at 60, $0.12 | 3 seats |

Each $1,000 of monthly income needs 107 seats or about 2,430 hosted reviews. INFERRED

Hosted tiers are blocked by audit R9 (the cap is per process, so one review can reach about $4) and R3 (the
published Action is behind `main`). DOCUMENTED

## 3. The model question: the test

Not answered here. 076p makes provider and model a setting after held-out scoring; frozen v1 is never edited.
DOCUMENTED

Confound: Sonnet's 17 of 31 against A0's 12 changed model, tools, prompt and lookup together (audit S2); Sonnet in
the unchanged pipeline scored 2 of 8 against 3. DOCUMENTED

Arms: identical skill bytes, no tuning, 6-comment cap. INFERRED

| Arm | What | Status | Tag |
|---|---|---|---|
| M0 | A0, frozen pipeline | Existing rows only | DOCUMENTED |
| M1 | Sonnet agent, short packet | Run: 17, 17 on dev | DOCUMENTED |
| M2 | Sonnet agent, no packet | Pre-registered `f827b23`, approved (076q), not run | DOCUMENTED |
| M3 | Nemotron Super agent, same skill and tools, by API | Not built; separates model from harness | INFERRED |
| M4 | Sonnet given title and repository name only | Not built; any catch is recall, not review | INFERRED |

Order (INFERRED): 1) commit plan and hashes; 2) M4, $0; 3) M2 on dev; 4) M3 smoke, 2 dev cases; 5) freeze every arm
by hash before the firewall lifts; 6) after the lift, one run of M3 and of M1 or M2 (A/B 2's rule picks) on the 29
opened held-out cases, two draws for Claude, one for Nemotron; 7) M3 on the 21 unexposed dev bugs if money remains.

Spend cap: Claude arms on Sunny's subscription (076m), never a product path (audit S6). Nebius $2.00 in total,
judging included, only from what H1 and H2 leave. M3 estimate $0.037 a run, from list prices and an unrecorded
90,000 input tokens. UNVERIFIED

Results, bench-judge-v1, mean of draws, held-out 20 (INFERRED):

- **Switch model:** Sonnet agent beats M3 by 4 or more on held-out and 3 or more on the 21 unexposed dev bugs; at
  most one extra comment on the 9 clean cases; M4 catches at most 1; measured cost at most $0.25 a review (076q).
- **Switch harness, keep Nemotron:** M3 beats A0 by 4 or more and Sonnet leads M3 by 3 or fewer.
- **No switch:** anything else; ties keep the incumbent (076).
- **Void:** M4 catches 2 or more; rerun only on cases fixed after every model's cutoff.

Training exposure: 0 of 31 dev fix commits are dated before 1 July 2026; 24 of 31 introducing commits are. The
cutoff is published to the month; earlier bug reports were not checked. DOCUMENTED. Held-out fix dates must come as
counts from the held-out lane. UNVERIFIED

## 4. Where the design still loses

Mechanism only; ranking INFERRED.

| # | Weakness | Tag | Cheapest test |
|---|---|---|---|
| 1 | The lookup addresses few bugs: 3 of 31 dev and 1 of 20 held-out are multi-file | DOCUMENTED | M2 against M1: approved, $0.15 |
| 2 | Volume. CodeRabbit and Greptile posted 123 and 114 counted comments on 40 dev PRs, A0 43; the skill caps at 6. More attempts catch more | DOCUMENTED | $0: recount catches with each tool cut to its first 1, 2, 3 comments per PR |
| 3 | Checking every finding removes true ones: verifiers lost three times and cut 10 of 32 true positives (058). Quoting two lines cannot express a missing check | DOCUMENTED | $0: audit T1.7's offline quote check |
| 4 | Contract facts rarely exist: the typed stage finished on 3 of 40 dev cases | DOCUMENTED | $0: serial rerun, 300 s budget |
| 5 | Compiler references miss string keys, events, injection, migrations, config and non-TypeScript files; a whole-repository index is not limited to symbols | INFERRED | $0, held-out lane: count labelled bugs inside the diff or affected set |
| 6 | No linters or SAST; CodeRabbit runs them on every paid plan | DOCUMENTED | $0: type-aware lint on dev case heads, count label hits |
| 7 | No learning from reactions (Greptile documents one); no users to learn from | DOCUMENTED | Proxy: comments on the 9 clean dev cases |
| 8 | Distribution: label trigger, same-repository PRs, key required, full re-review each trigger | DOCUMENTED (audit R6, R7) | $0: one timed scratch-repository dry run |

## 5. Kill criteria

Constraints: job hunt, near-zero budget, 50 stars by 31 Dec. Stars today: 5. VERIFIED (`gh repo view`)

| Date | Evidence that must exist | Otherwise |
|---|---|---|
| 25 Nov | Held-out score files hashed; band computed by task 1 | Band C from 30 Nov |
| 30 Nov | The write-up public, whatever the result | Nothing else starts |
| 7 Dec | §3 steps 2–6 done within $2.00 | No switch this year |
| 15 Dec | Bands A, B: one repository Sunny does not own has run a review on a real PR; 25 stars | Company work stops; open source only |
| 15 Dec | Band B: task 8's gate passed | Claim dropped; band C |
| 31 Dec | Band A: one paying user or written commitment to pay | Company work stops; open source only |
| 31 Dec | 50 stars | Goal missed; company work is not reopened |

Standing limits (INFERRED): no Nebius top-up; no paid run with the console under $1.00 ($16.35 on 16 Nov,
DOCUMENTED); from 1 Dec at most 10 hours a week, job hunt first.

## 6. Next four weeks

Lanes as in `AGENTS.md` (builder's branch, GO on a sha, orchestrator merges). New paths only until the held-out tag;
check the scheduler's pins before merging. All INFERRED.

| # | Week | Bands | Task and files | Test | Accept | Rollback |
|---|---|---|---|---|---|---|
| 1 | 1 | All | Band calculator over counts-only JSON: `scripts/product/band.ts`, `packages/analyzer/test/product-band.test.ts` | `pnpm --filter @tracehound/analyzer exec vitest run test/product-band.test.ts` | 14 fixtures cover every edge and modifier in §1 | Delete both |
| 2 | 1 | All | Register §1: decision entry quoting this file's sha256 | `shasum -a 256` matches | Committed before the score-hash commit | `git revert` |
| 3 | 1–2 | A–C | Run A/B 2 per `sidetrack/ab2-plan.md`; keep per-case usage outside the repo | The plan's scoring command | Its decision line applied as written; one usage file per case | Report only |
| 4 | 2 | A–C | M3, M4: `scripts/track-c/review-case-api.ts`, `packages/analyzer/test/track-c-api.test.ts`; `llm/client.ts` unchanged | `pnpm --filter @tracehound/analyzer exec vitest run test/track-c-api.test.ts` | Mocked; skill sha equals M1's; turn, token, wall caps; smoke under $0.10 | Delete both |
| 5 | 2 | All | Write-up, results tables, README | `pnpm --filter @tracehound/viewer test` | Counts of 20 with intervals, both pairs, exposure split, predictions scored, band line | `git revert` |
| 6 | 3 | A–C | Audit T2.3 (pin bump), then T2.1 (retry) | As the audit | As the audit | As the audit |
| 7 | 3 | A–C | Audit T2.9 (skill for a user's own Claude Code); A, B also T2.8 (provider setting) | Mocked, per provider | One scratch-repository dry-run review | Default stays `nebius` |
| 8 | 3–4 | B | Seeded cross-file set (audit E2): 30 seeds, 10 controls, never `tracehound-seeds` | Plan committed before the first review | Lookup arm catches 12 or more of 30, and 6 more than no-lookup | Drop the claim |
| 9 | 4 | A–C | Launch post; README top: result, method, three-line install | `gh repo view SunnyBagal/TraceHound --json stargazerCount` | Count recorded 15 and 31 Dec | None |
| 10 | 4 | A | One cap per review across processes (audit R9), as a new reviewer version | Three subprocesses plus the fix client cannot exceed one cap | No hosted review over $1 | Flag off |

Band D runs tasks 1, 2 and 5, then stops.
