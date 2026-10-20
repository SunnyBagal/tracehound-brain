PR #54 (draft): docs: README for judges (placeholders for numbers). Branch docs/judges-readme from origin/main (2a95756). Head db83286706022cd74aafb7d566a948f1d40a68b7.

Commits:
- db83286 readme: top section for hackathon judges, placeholders for numbers

Files changed: README.md (+109, 0 deleted). The old README is byte-identical below `---` / `## Details` (checked with cmp).

Tests (plain shell, GIT_* env check empty): pnpm test passed. Action 179 passed; analyzer 859 passed, 7 skipped; viewer 188 passed. pnpm typecheck passed in all 3 workspaces.

Placeholders left for the chat: [[RESULTS-TABLE]], [[REVIEWBENCH]], [[SEEDED-TEST]], [[COST]]. No benchmark numbers are typed in the new section.

Deviations and notes:
- docs/action/README.md has no YAML block. The try-it block is the review job of docs/action/example-workflow.yml, with the data job and push trigger removed, as its setup step 3 says.
- I added `model: nvidia/nemotron-3-super-120b-a12b` to the try-it block. With an empty model input, the Action runs both reviewers on Nano (DEFAULT_MODEL), and only the verifier and fixes use Super. The benchmark used Super.
- Decision 067 isn't on main (only on build/067-dev-3.5), so "being evaluated (decision 067)" has no link.
- The unchanged Details section still has the old "PR reviewer benchmark" paragraph and table with numbers, plus a second "# TraceHound" heading.
- Contracts appear in the diagram and text as "landing in int-01" (draft PR #53). Update this when #53 merges.

Not done: no live check of the Mermaid rendering on GitHub. No model or Nebius calls were made.

---

## Bunny's chat reply (verbatim, as relayed by Sunny)

I rewrote the top of the README and opened it as draft PR #54, "docs: README for judges (placeholders for numbers)". Tests and typecheck pass, and the report is pushed to the brain inbox.

- Branch: docs/judges-readme, from origin/main at 2a95756. Head is db83286.
- Commit: db83286 readme: top section for hackathon judges, placeholders for numbers. It changes only README.md (109 lines added).
- Old README: it sits unchanged below a --- divider under ## Details. I checked with cmp that it is byte-for-byte the same.
- Tests: pnpm test passed with 1,226 passed and 7 skipped (action 179; analyzer 859 passed, 7 skipped; viewer 188). The git environment check before it printed nothing.
- Typecheck: pnpm typecheck passed in all 3 workspaces.
- Placeholders for the chat: [[RESULTS-TABLE]], [[REVIEWBENCH]], [[SEEDED-TEST]] and [[COST]]. I typed no benchmark numbers in the new section.
- Inbox: the report is in inbox/2026-10-20-bunny-readme.md in the brain repo, pushed as 88d9676.

Things the chat should know when checking it:
1. The "try it" YAML isn't copied word for word. docs/action/README.md has no YAML block, so I used the review job from docs/action/example-workflow.yml and removed the data job and push trigger, as the docs' setup step 3 says. I also added one line: model: nvidia/nemotron-3-super-120b-a12b. When model is left empty, the Action runs both reviewers on Nano, and only the verifier and the fix suggestions use Super. The benchmark ran on Super, so without that line the YAML wouldn't match "Built with Super".
2. Decision 067 has no link. It is only on build/067-dev-3.5, not on main, so the README just says "being evaluated (decision 067)".
3. Benchmark numbers still appear lower down. The unchanged Details section keeps the old "PR reviewer benchmark" paragraph and table, and a second # TraceHound heading. You asked for that part to stay as it was.
4. The contracts text needs an update when #53 merges. The diagram and text mark cross-file contracts "landing in int-01" (dashed in the diagram).

I didn't check that the Mermaid diagram renders on GitHub. I made no model or Nebius calls.
