# codex-17 — decision 072 Action pin refresh (2026-10-28)

Completed, docs only plus the explicitly requested demo workflow pin. Model calls: 0. Model spend: $0.

## Published heads

- PR #54: https://github.com/SunnyBagal/TraceHound/pull/54 — `a69d0d0f8e62d24ab8947875b012b0049c43e25a` (`docs/judges-readme`).
- PR #58: https://github.com/SunnyBagal/TraceHound/pull/58 — `1ea041b7859316ae3879083812d5b955159d16ed` (`docs/devpost-drafts`).
- Demo default branch `main`: https://github.com/SunnyBagal/tracehound-demo/commit/c3ae1282cf557dbfa8c14410b800704b89e1c3dd — `c3ae1282cf557dbfa8c14410b800704b89e1c3dd`.
- Demo commit subject: `ci: pin TraceHound review-freeze-2.1 (decision 072)`.

Both docs branches merged `origin/main` (`867c823`) first. Both merges completed without conflicts. No history rewriting or PR merging. Demo fetched only default-branch main; no demo PR was opened, closed, read or modified, including #6.

## Step 0

- `git fetch origin --tags`: passed.
- `git rev-list -n 1 review-freeze-2.1`: `86fd5d796c901cb20177cda28c4925e8c72d0901`.
- `git diff --stat 0aeef06 review-freeze-2.1`: only `.github/actions/tracehound-review/action.yml`, 5 insertions and 4 deletions.
- `origin/main` contains decision 072; read that decision.
- Pulled brain with `git pull --rebase` before reading the current numbers.md §0 rule 5. Used its wording once at the README's first product-config statement: “The review pipeline at `review-freeze-2`; the Action at `review-freeze-2.1` fixes installation only.”

## Every replaced line (old → new)

The blocks below list every removed line and its replacement, grouped by diff hunk. Added lines are included with their surrounding replacement. Each arrow separates old from new.

### README.md (#54)

README.md

```text
      # 0aeef06… is the commit tagged review-freeze-2, the frozen release that was measured.
→
      # 86fd5d7 is the commit tagged review-freeze-2.1: the measured review pipeline of
      # review-freeze-2 (0aeef06) with only the install step changed (decision 072).
```

README.md

```text
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2
→
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@86fd5d796c901cb20177cda28c4925e8c72d0901 # review-freeze-2.1
```

README.md

```text
called TraceHound v2 here. Benchmark: `ai-code-review-evaluations/golden_comments` (5 repos × 10
→
called TraceHound v2 here. The review pipeline at `review-freeze-2`; the Action at
`review-freeze-2.1` fixes installation only. Benchmark: `ai-code-review-evaluations/golden_comments` (5 repos × 10
```

README.md

```text
to `@0aeef06` with the 3.5 model set explicitly. On demo PR #2, the current config posted no
→
to `review-freeze-2.1` (`86fd5d7`) with the 3.5 model set explicitly. On demo PR #2, the current config posted no
```

### Devpost files (#58)

docs/devpost/video-script.md

```text
`0aeef06` (tag `review-freeze-2`), with the product model set explicitly, as in the README.
→
`review-freeze-2.1` (`86fd5d7`), with the product model set explicitly, as in the README.
```

docs/devpost/video-script.md

```text
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2
→
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@86fd5d796c901cb20177cda28c4925e8c72d0901 # review-freeze-2.1
```

docs/devpost/writeup.md

```text
pinned to `0aeef06` with the 3.5 model set explicitly; on demo PR #2 that config posted no findings
→
pinned to `review-freeze-2.1` (`86fd5d7`), the measured pipeline with an install fix,
with the 3.5 model set explicitly; on demo PR #2 that config posted no findings
```

docs/devpost/writeup.md

```text
(no line; insertion)
→
- **Our own Action failed before review.** The seeded test caught an install bug: on a repo
  pinning another pnpm version via `packageManager`, the setup step failed before any model call.
  We fixed it as `review-freeze-2.1`, a one-file change with the review code unchanged (decision 072).
```

### Demo .github/workflows/tracehound-review.yml

.github/workflows/tracehound-review.yml

```text
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2
→
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@86fd5d796c901cb20177cda28c4925e8c72d0901 # review-freeze-2.1
```

## Remaining grep results

Equivalent to `grep -rn 0aeef06 README.md docs/devpost/writeup.md docs/devpost/video-script.md`, with README read from #54 and the two Devpost files from #58:

```text
README.md:46:      # review-freeze-2 (0aeef06) with only the install step changed (decision 072).
README.md:92:(`nvidia/Nemotron-3.5-Super-VL`) at tag `review-freeze-2` (`0aeef06`)**, via Nebius Token Factory,
docs/devpost/writeup.md:80:(`nvidia/Nemotron-3.5-Super-VL`) at tag `review-freeze-2` (`0aeef06`), via Nebius Token Factory.
docs/devpost/writeup.md:159:  065, 067), and freeze tags (`eval-freeze-2`, `review-freeze-1`, `review-freeze-2` → `0aeef06`).
docs/devpost/video-script.md:9:(`0aeef06`).
```

All five are deliberate `review-freeze-2` references: install-fix ancestry, measured README config, measured Devpost config, preregistered freeze-tag history, and measured video pipeline config respectively. None is an install pin.

## Validation and files

- `git diff --check`: passed for both docs changes and the demo workflow.
- Pre-gate environment check printed nothing before each gate; gates ran in a plain shell.
- `pnpm test`: passed on the final #54 working tree before commit: 1,441 passed, 8 skipped, 0 failed; 96 test files passed, 6 skipped. Action: 249 passed; analyzer: 1,004 passed, 8 skipped; viewer: 188 passed.
- `pnpm typecheck`: passed in all 3 workspace packages (action, analyzer, viewer), exit 0.
- #58 has identical executable/test/config content to the tested #54 branch: the complete diff contains only README.md and docs/devpost/ files. Gates were not repeated for identical code.
- README: all 33 Markdown link targets unchanged, all 3 internal anchors resolve, all 22 local paths exist; both `[[TBD-SEEDED]]` tokens preserved.
- Devpost: all 6 seeded/Sunny placeholders in writeup.md and all 4 in video-script.md preserved exactly; Markdown links unchanged. Added challenge bullet names no reviewer or coding tool.
- Demo: exactly one uses line changed; `model: 'nvidia/Nemotron-3.5-Super-VL'` preserved. Workflow has only pull_request triggers, so this main-branch pin push does not trigger its review job.
- Authored files: README.md (#54); docs/devpost/writeup.md and docs/devpost/video-script.md (#58); .github/workflows/tracehound-review.yml (demo); inbox/2026-10-28-codex-17.md (brain report). Other branch files came solely from the requested origin/main merges.
- Remote heads verified after pushing. Worktrees clean before report publication.
- Nothing outstanding in the requested scope. Seeded and Sunny placeholders intentionally remain; no evaluation, new model request, or demo PR action was performed.
