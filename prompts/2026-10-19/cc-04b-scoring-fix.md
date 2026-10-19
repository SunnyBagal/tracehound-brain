# cc-04b: fix the scoring rule and add CodeRabbit (before the hashes are published)

Tool: **Claude cloud**, the **same session** that built `tracehound-seeds` (head `e98720d`). Reply in that session with everything below the line.

---

Good work. You were right about rule (a): it can't fire for any X seed, because GitHub only takes inline comments on files in the diff. That's my mistake in SCORING.md. Nothing has been published yet, so we change it now. Push to `tracehound-seeds` `main` only. Don't touch the mirrors (no branch changes, no force-push).

## 1. Replace the caught rule in `SCORING.md`
Replace the "caught" definition with this text, exactly:

> A seed counts as **caught** by a tool if any comment by that tool on that PR satisfies the rule for its class. "Comment" means inline review comments, review bodies, and PR conversation comments.
> - **D seeds (location rule, automatic):** an inline comment on an `accept` path at a line within `[start-3, end+3]`.
> - **X seeds (mention rule, automatic):** a comment whose text contains the `accept` path or its file basename **and** `brokenSymbolOrKey` (case-sensitive substring), where the comment is anywhere on the PR. A comment that only restates the PR's own change does not count unless it also names the unedited file.
> - **Manual rule, every class (Sunny):** a comment that clearly identifies the defect without meeting the automatic rule, for example a paraphrase of the key, or naming the caller without its path. Every manual call is listed with the comment quoted and a one-line reason. Manual calls are reported in their own column, so the automatic-only score is always visible.
> - **C controls:** report the number of comments per tool. Sunny marks each one valid or invalid. Any invalid comment on a control is a false alarm.
> - Report caught / n per tool and per class under the automatic rule alone and under automatic + manual, plus the extra comments on seeded PRs. No LLM judge.
> - Tools: TraceHound (frozen at freeze-2), CodeRabbit (OSS plan), Greptile (Starter), and Bugbot.

Keep the rest of SCORING.md as it is.

## 2. `score.ts`
- Implement the rules above. Read inline comments (`pulls/{n}/comments`), reviews (`pulls/{n}/reviews`) and conversation comments (`issues/{n}/comments`).
- Default logins:
  - TraceHound: `github-actions[bot]`. It posts through the workflow's `GITHUB_TOKEN`; keep `TRACEHOUND_LOGINS` as an override.
  - CodeRabbit: `coderabbitai[bot]`.
  - Greptile: `greptile-apps[bot]`.
  - Bugbot: `cursor[bot]`.
  - Each one can be overridden by an env var. The script prints the logins it used.
- Output one table with columns for automatic, manual and both. Manual calls are read from a `manual.json` file that Sunny writes. Make it an empty array for now, plus a documented schema.
- Add offline fixture tests:
  - an X seed caught by mention in a review body;
  - an X seed **not** caught by a comment naming only the symbol;
  - a D seed caught by location;
  - a D seed missed at a line outside ±3;
  - a CodeRabbit walkthrough comment that restates the diff, which must not count;
  - a control with 2 comments.

## 3. `open-prs.sh`
- After each `gh pr create`, post `gh pr comment <url> --body "@coderabbitai review"`. CodeRabbit's OSS plan needs a manual trigger on repos with fewer than 10 stars.
- Pace it at most **8 PRs per hour** in total. Keep the shuffled order and the recorded seed.
- Keep the `--dry-run` flag, which prints the commands without running them. Don't run the script for real.

## 4. Commit authorship (leave it)
The Claude author and co-author lines on the mirror commits stay as they are. Every tool sees the same commits, and AI-authored PRs are normal now. Don't force-push.

## 5. README
Add one line: until Sunny says scoring has started, nobody working on TraceHound may open `linea-seeded`, `recall-seeded` or `tracehound-seeds`.

End with: the new `sha256sum key.json SCORING.md`, the score.ts fixture results, `open-prs.sh --dry-run | head -12`, and the head sha.
