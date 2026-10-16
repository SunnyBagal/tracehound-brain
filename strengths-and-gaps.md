# Strengths and gaps

An honest ledger, updated after each result. Last updated: 2026-10-16, after 059.

## Where we are good or better

| Strength | Evidence |
|---|---|
| **Measurement honesty** | Pre-registered bars and freeze commits (058, 059). Four losses recorded as losses (050, 056, 057, 058). One judge for every reviewer. A blind test set, run once |
| **Recall at a commercial level** | diff-v1 finds 32 of 74 dev defects, the same as CodeRabbit and more than Cursor (29) or Copilot (28). The raw pool (059) finds 38, more than Greptile (35); only Augment (41) finds more |
| **First variant over a pre-set bar** | pool-verify-v1 F 35.6% (bar 35.0), adopted for the freeze; above Copilot (31.3) on dev |
| **Noise now at Greptile's level** | verify-ctx-v1 leaves 0.82 wrong comments per PR, equal to Greptile and below CodeRabbit (1.25) and Copilot (1.93) |
| **A verifier that separates** | verify-ctx-v1 kept 69% of real findings but only 33% of false ones |
| **Cost** | About 1–2 cents per PR for the reviewer, under a cent per PR for the verifier, on our (estimated) ledger. Under $12 spent on Nebius across the whole project so far |
| **Open stack** | Open model (Nemotron), MIT code, self-hostable GitHub Action |
| **Engineering depth** | A Docker sandbox with tested isolation; a deterministic, evidence-backed graph; CI with tests and typecheck on every PR; spend caps and a ledger on every call |
| **Process** | Parallel lanes (relay, Codex) on paths that don't overlap; every PR checked from GitHub; Grok audits that cite file and line |

## Where we fail or are behind

| Gap | Evidence | What is being done |
|---|---|---|
| **Precision** | diff-v1: 74% of comments wrong, the worst of any reviewer measured | 058 filters; 059 pools, then filters |
| **Every precision fix costs recall** | 056–058: F stuck at 31.8–32.8%. 059 got to 35.6% by widening the pool, but the verifier still drops 12 of 38 real findings | Next: scorer checks (060) before more tuning |
| **Thin margin, selection on dev** | 059 cleared the bar by 0.6 points (one judge flip → 34.2), on the fourth variant tried on the same 40 PRs | Only Cal.com confirms; report diff-v1 and pool-raw beside it, declared in advance |
| **Same model reviews and judges** | Reviewer and judge are both Nemotron Super; a judge can favour outputs from its own model | 060: a second judge from a different model family |
| **Far behind Augment and Cursor** | Augment F 64.1%, Cursor 49.6%, Greptile 49.3% vs our best 32.8% | Pitch honesty and openness, not rank ([P004](decisions/P004-target-reset.md)) |
| **No live reviewer demo yet** | The Action is merged but has never run on a real repo | Codex demo repo, 17–18 Oct |
| **Golden set biased against new reviewers** | Augment extended it with the five tools' comments | 060 augmented precision |
| **One judge** | Error not measured | 060 second judge |
| **The graph didn't help** | 050: 0 graph tool calls; S1: 2.1% pairing on unseen repos | Graph kept as a viewer and for context ideas; not the reviewer's engine |
| **Brief quality (chat)** | 057's budget cut the thing under test; a fix prompt targeted a merged branch; a CI prediction was wrong | [lessons](lessons.md) |
| **Unverified prices** | `config/prices.json` uses third-party rates | Confirm in the Nebius console before quoting costs publicly |

## What would change the picture

- 059 clears F 35%: the first reviewer better than the baseline, at about 2–5 cents per PR.
- 060 shows that our "false positives" are often valid findings the golden set lacks: precision rises for us more than for the tools.
- The demo shows readable, useful comments on real PRs: this is what the judges score.
