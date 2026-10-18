# cc-01: independent audit of the benchmark claims (Claude cloud session)

Tool: **Claude Code on the web (claude.ai/code), a new cloud session on repo `SunnyBagal/TraceHound`, branch `main`.** Paste everything below the line. This uses the Claude cloud credit. **No Nebius key is in this session, and none is needed: make no model calls through our code.**

---

You are an independent auditor. You have not seen how these results were produced; that is the point. Audit the public benchmark claims in this repo as a hostile but fair reviewer would, for example someone replying on X who wants to prove us wrong.

Read:
- `README.md` (section "PR reviewer benchmark");
- `docs/bench/results.md`, `docs/bench/test-results.md`, `docs/bench/test-plan.md`;
- `docs/bench/scorer-validation.md`, `docs/bench/scorer-checks.md`, `docs/bench/pool-verify.md`;
- decisions 054–063 in `docs/decisions.md`.

Do:
1. Recompute every P/R/F in those docs from their counts, and list any mismatch with file:line.
2. Check that every claim in README and results.md is supported by a table in the repo. Quote each unsupported or overstated sentence.
3. Compute what a critic would compute:
   - all 50 PRs combined (dev + Cal.com run 1) for every reviewer;
   - a bootstrap 95% CI over PRs for the Cal.com F difference between TraceHound and each tool, if per-PR counts are in the repo; if they aren't, say so and give the best bound you can from the aggregate counts.
4. List the threats to validity a critic will raise, ranked by severity. For each, say whether the docs already disclose it (with file:line).
5. Write `docs/bench/audit-cc01.md` with all of the above. Then open a **draft** PR titled `docs: independent audit of benchmark claims (cc-01)`.

Rules:
- **Cal.com is blind.** Do not fetch any Cal.com PR, golden comment, or tool review from the internet. Use only counts already in this repo.
- Only touch `docs/bench/audit-cc01.md`. One commit.
- Never merge. End with a summary: mismatches found, unsupported claims, the 50-PR table, and the top 3 threats.
