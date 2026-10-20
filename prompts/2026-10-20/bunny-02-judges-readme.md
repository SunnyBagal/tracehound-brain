# Bunny job 2: judges' README draft, with placeholders for the numbers

Where: **Bunny** (`claudeb`), a **new** session, `~/Projects/TraceHound-claude2`. **Start it after bunny-066b's PR is open.** Use the strongest model (it's writing that judges will read). $0, no model calls. Paste everything below the line.

---

You are a builder on TraceHound, an open-source AI PR reviewer. Rules:
- Work only in `~/Projects/TraceHound-claude2`. Read `CLAUDE.md` and `AGENTS.md` first.
- Never open `SunnyBagal/linea-seeded`, `recall-seeded`, `tracehound-seeds`, or ReviewBench's `golden/`.
- Make no model or Nebius calls. Open PRs as drafts. Never merge or force-push.
- Run the tests as their own command, read the result, then commit separately.
- When done, write your final report verbatim to `~/Projects/tracehound-brain/inbox/<date>-bunny-readme.md` (rules in `inbox/README.md`) and push it.

**Task:** rewrite the top of the repo's `README.md` for **hackathon judges** (Nebius × NVIDIA, Coding and Agentic Engineering track). A judge may give it 2 minutes. Branch `docs/judges-readme` from `origin/main`.

Structure:
1. **One line:** what TraceHound is. An open-source GitHub Action that reviews pull requests with NVIDIA Nemotron on Nebius Token Factory.
2. **A 20-second "try it" block:** the minimal workflow YAML from `docs/action/README.md`.
3. **How it works:** a short diagram (Mermaid) of the real pipeline as it runs **today**:
   - two generators (diff-only and repo-context);
   - pool → verifier;
   - deterministic cross-file contracts (marked "landing in int-01" until that PR merges);
   - learning from maintainer feedback (`learn` input, off by default).

   Link the decision log for the stages we tried and dropped (064 vote, 065 evidence gate).
4. **Results, honestly:**
   - a table placeholder `[[RESULTS-TABLE]]`;
   - one line placeholder `[[REVIEWBENCH]]`;
   - one line placeholder `[[SEEDED-TEST]]`.

   Under them, the fixed rule: "every number links to its run record; every loss is published". Link `docs/bench/results.md`. **Don't type any benchmark number.** The chat fills these after the runs.
5. **Cost per PR:** a placeholder `[[COST]]`.
6. **Built with:** Nemotron 3 Super via Token Factory (link `config/prices.json` for prices). Mention Nemotron 3.5 Super only as "being evaluated (decision 067)".
7. **What's next:** 3 bullets at most, no promises with dates.

Keep the existing README content below a `---` divider under "Details", unchanged. No marketing words ("revolutionary", "best", "state of the art", "beats"). Open the PR as a draft, titled `docs: README for judges (placeholders for numbers)`.
