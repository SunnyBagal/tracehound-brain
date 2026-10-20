```
 Ran git status --porcelain; git rev-parse HEAD
  └ 747a3a79dd917e572e2350196fdaa379b2bc5f51

```

Draft PR #53 (https://github.com/SunnyBagal/TraceHound/pull/53) is ready. CI is green.
Head: `747a3a79dd917e572e2350196fdaa379b2bc5f51`

* `55f2d73` — Add contracts input, defaulting on.
* `a9d2f05` — Add bounded runner with merge-base and credential-free git handling.
* `f7347cd` — Deliver contracts alongside model findings, with separate caps and footer counts.
* `747a3a7` — Add real-checker integration tests and documentation.

`pnpm test`: 1,259 passed, 7 skipped — Action 214, analyzer 859, viewer 186. `pnpm typecheck`: 3/3 workspaces passed.
`contracts: 'off'` is byte-identical, tested for both reviewers and fixes modes. All 13 changed files (https://github.com/SunnyBagal/TraceHound/pull/53/files) are in scope.
The live review (https://github.com/SunnyBagal/tracehound-demo/pull/6#pullrequestreview-5587521890) lists four untouched dependents, including `recall-backend/index.ts:13` and `recall-backend/worker.ts:10`, with head-SHA links. Contracts completed successfully in 2.255 seconds; the complete review job took 32 seconds. One normal review was run.
Nothing remains unfinished.
