# bunny-05 report (23 Oct): #54 install/contracts/model, contracts boundary (#61), #58 sentence

Step 0 passed. Fetched origin and tags, `git status --porcelain` was empty, origin/main contains 5fb8cdc (#59), the GIT_* env check printed nothing, and `pnpm test` / `pnpm typecheck` were green on origin/main 5fb8cdc. Tag `review-freeze-2` → 0aeef06.

## #54 (docs/judges-readme, draft): head 1c57c720c3d2164c13116e616854c77781347cde
- 74daf9c Merge origin/main into docs/judges-readme (a `--no-ff` merge commit; no rebase or force-push)
- 1837d4c readme: pin review-freeze-2, drop the model line (068 default). The pin is now `@review-freeze-2`. The comment says the default runs every review stage on Nemotron 3.5 Super VL and fixes run on Nemotron 3 Super.
- cdea107 readme: contracts are merged (#53), drop int-01 and the what's-next item. Changed the text and the Mermaid node, and the bullet now links docs/contracts.md.
- bece60f readme: Nemotron 3.5 Super adopted (067) and the default (068). "Built with" now reads: "adopted on 2 dev runs: v1 F 50.4 vs 42.3; the second judge's interval includes 0", then 068's default with partial reviews when a chunk fails. 3 Super is now listed for fixes only. Removed "Decide on 3.5" from What's next.
- 52f1964 readme: drop the second '# TraceHound' H1 under Details
- 1c57c72 readme: [[CALCOM-V2]] placeholder next to the results (decision 069). [[RESULTS-TABLE]], [[REVIEWBENCH]], [[SEEDED-TEST]] and [[COST]] are kept, one of each.

## #61 docs: contracts security boundary (codex-13 audit) (docs/contracts-boundary, draft): head eef06044df93bec633d512d6056676319ede910f
- 03e8a28 action docs: bound the contracts claim (codex-13 audit). The text now says the stage installs nothing, runs none of the PR's scripts and loads no TS plugins. It also says the stage parses the PR's tsconfig.json and type files with the bundled TypeScript, and those can reference files outside the checkout. Contracts are for same-repo PRs only, forks are skipped, and the stage is not a sandbox.
- 057d2f3 contracts docs: the node_modules symlinks are not read-only (codex-13 audit)
- eef0604 action docs: Security boundary section (codex-13 audit). It covers: same-repo only; forks skipped in action.ts; why; what the contracts stage reads; and the backlog (restrict compiler reads to the checkout, an env allowlist).

## #58 (docs/devpost-drafts, draft): head ff483fc3e2e37ac08cd038dd91ccf44e55483898
- ff483fc docs(devpost): bound the contracts claim (codex-13 audit), noted under item 11. writeup.md "What it does" now uses the same bounded wording as #61, and checklist item 11 quotes it.

## Tests
- Gates ran as their own commands before every commit and were green: action 237; analyzer 890 passed + 7 skipped (880 + 7 on docs/devpost-drafts, which is behind main); viewer 188; typecheck green.
- One timeout rerun, before 1c57c72 on #54: `test/contracts-cli.test.ts` › "writes the report for a pull request and prints a summary" hit "Test timed out in 5000ms". `pnpm exec vitest run test/contracts-cli.test.ts --testTimeout 30000` then passed 3/3. The viewer tests skipped by that failure ran on their own and passed 188/188.

## Mermaid
- Rendered with `npx -y @mermaid-js/mermaid-cli` 12.0.0 (mmdc). Exit code 0, the SVG has no "Syntax error", and it shows all 8 edges, including the contracts node.

## Couldn't do / notes
- In writeup.md I kept "no model, no network" next to the new wording. The analyzer makes no network calls, but the Action's contracts stage can fetch a missing base commit without credentials (docs/action/README.md). Sunny should decide whether "no network" stays.
- CLAUDE.md still says worktrees get "read-only symlinks" (Change sets). It wasn't in this job's files, so it is unchanged.
- In #54, "v1 F 50.4 vs 42.3" is the brief's wording, verbatim. The 42.3 is 3 Super's 5-run dev mean (067).
