# cc-04b output (pasted by Sunny 19 Oct 17:43). Checked by chat: rules match the brief; hashes recorded in NOW as the pre-registration.

Checking one detail first: for the X mention rule, a basename like `index.ts` could match both an unedited accept file and a file the PR edited.

No collisions in the real key, but `score.ts` will still require the full path when a basename collides, so a summary that only restates the PR's own edit can never count. Writing `SCORING.md`:

Now rewriting `score.ts` for the new rules.

Now the fixtures, covering the six cases you listed plus manual calls and control marks.

13/13 pass. Running the full fixture report and a run against the real key (no PRs yet, so everything should read missed):

Now `open-prs.sh`: adding the CodeRabbit trigger comment and slowing the pace to at most 8 PRs per hour.

Adding a guard so a re-run after an interruption also respects the 450s spacing from the last PR opened:

Dry run is fine and the order is unchanged. Now the README: the access line, the new scoring section, and schemas for `manual.json` and `control-marks.json`.

All five changes are pushed to `tracehound-seeds` `main`, and the scorer's 13 offline checks pass. The mirrors are untouched: both `main` branches are unchanged (`4f075a5`, `20ac149`) and each still has its 15 branches plus `main`.

- **`SCORING.md`:** your text is in exactly. I replaced the whole file rather than keeping the rest, because every old line was covered by your new text or contradicted it. The old lines referred to rules (a)/(b), and the old tools line excluded CodeRabbit.
- **`score.ts`:**
  - It implements the D location rule and the X mention rule, reading all three comment sources.
  - Default logins are `github-actions[bot]`, `coderabbitai[bot]`, `greptile-apps[bot]` and `cursor[bot]`. Each can be overridden by an env var, and the script prints which logins it used.
  - Output is one table per tool and class with three columns: `automatic`, `manual` (seeds caught only by a manual call) and `auto + manual`.
  - `manual.json` starts as an empty array; its schema is in the README.
  - To report false alarms on controls I also added `control-marks.json` (empty), where you record valid/invalid for each control comment.
  - If a basename is shared with a file the PR edits, only the full path counts. That way a summary that only restates the PR's own change can never match.
- **`open-prs.sh`:** it posts `@coderabbitai review` after each PR and opens one PR every 450s (8 per hour), so the full run takes about 3h40m. The spacing also holds if you re-run it after an interruption. The shuffle seed and order are unchanged. I only ran `--dry-run`.
- **README:** the access line is added, along with the new scoring rules and the two file schemas.

Two things to check before 25 Oct:
- **CodeRabbit's terms:** the old `SCORING.md` excluded CodeRabbit because its terms forbid publishing benchmark results. Please confirm the OSS plan's terms allow it before you publish.
- **TraceHound's login:** `github-actions[bot]` is shared by every Actions workflow. If anything else on the mirrors comments through Actions, it would be counted as TraceHound. Each mirror only has `main` plus the 15 branches, so this only matters if you add workflows.

**Hashes**
```
abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1  key.json      (unchanged)
ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d  SCORING.md
```

**score.ts fixture results** (`bun test score.test.ts`): 13 pass, 0 fail. They cover:
- An X seed caught by mention in a review body.
- An X seed not caught by a comment naming only the symbol.
- A D seed caught at +3 lines.
- A D seed missed at +4 lines; a review body naming the file and key doesn't count for D either.
- A CodeRabbit walkthrough restating the diff (key plus an edited `index.ts`) not counted.
- A control with 2 comments: one marked invalid (a false alarm), one unmarked. An empty review and your own trigger comment are ignored.
- Base-side comments, a seed with no PR, manual calls counted in the manual column only, input checking for both JSON files, login overrides, and the summary and extra-comment counts.

**`./open-prs.sh --dry-run | head -12`**
```
Opening 30 PRs in this order (SHUFFLE_SEED=20261025, one every 450s):
   1. s15  SunnyBagal/recall-seeded  refactor/chat-sources-event
   2. s29  SunnyBagal/linea-seeded  fix/clear-selection-on-remote-update
   3. s25  SunnyBagal/recall-seeded  feat/abortable-text-extraction
   4. s07  SunnyBagal/recall-seeded  fix/ai-summary-parse-failure
   5. s22  SunnyBagal/linea-seeded  refactor/pencil-segment-helpers
   6. s10  SunnyBagal/recall-seeded  feat/dedupe-saved-links
   7. s08  SunnyBagal/linea-seeded  refactor/move-socket-hook
   8. s14  SunnyBagal/recall-seeded  feat/metadata-timeout-option
   9. s11  SunnyBagal/linea-seeded  fix/ops-load-auth-expiry
  10. s16  SunnyBagal/linea-seeded  refactor/chat-broadcast-event
  11. s24  SunnyBagal/recall-seeded  refactor/card-variant-prop
```

**`tracehound-seeds` head:** `a4a6c8349626301bfcf55700c3a6063b4aed47b4`