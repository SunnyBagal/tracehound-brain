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
