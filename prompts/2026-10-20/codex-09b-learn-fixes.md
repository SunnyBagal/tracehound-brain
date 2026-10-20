# codex-09b: fix the learning matcher on #49

Tool: **ChatGPT Codex**, the **same Codex task** that opened #49 (branch `codex/learn-v1`), in `~/Projects/TraceHound-codex`. **Must land by 22 Oct.** If that task is gone, open a new one on `codex/learn-v1`. Paste everything below the line.

---

The chat checked PR #49 at `30a3d44`. All paths are in scope, the analyzer learn tests (47) and action tests (174) pass, `learn: 'off'` is unchanged, and the maintainer permission check is right.

There's one real defect, and it comes from the original brief, not from you. `fingerprint()` drops code spans, so identifiers vanish. Probe results:

| Rejected: "Possible null dereference: \`user\` may be undefined in getProfile" (`src/user.ts`) | Today |
|---|---|
| Same file: "Possible null dereference: \`order\` may be undefined in getProfile" (a different bug) | **suppressed** (wrong) |
| `src/auth/session.ts`: "Possible null dereference: \`token\` may be undefined in \`validate\`" | **suppressed** (wrong) |

Reviewers put identifiers in backticks almost every time, so one 👎 would silence that whole class of bug across every file with the same extension. Fix it on `codex/learn-v1`, with tests, keeping `pnpm test` and `pnpm typecheck` green. Same path rules as codex-09. **Don't open** `linea-seeded`, `recall-seeded` or `tracehound-seeds`.

1. **Identifiers.** `fingerprint()` also returns `ids`: the contents of code spans, split on anything other than `[A-Za-z0-9_]`, lowercased, keeping only parts of 2 or more characters, as a sorted set. Prose `tokens` stay as they are now.
2. **`similar()`:**
   - If both sides have `ids` and they share none → **not similar**.
   - The same-extension rule, which matches across files, also needs **at least one shared id**. If either side has no `ids`, only the same-path rule can match.
   - Thresholds are unchanged.
3. **Stored data.** `feedback/rejected.json` entries gain `ids`. An entry without `ids` (older files) is read as `ids: []`, so it can only match on the same path. Note this in `docs/learn.md`.
4. **New fixtures** in `packages/analyzer/test/learn.test.ts`, alongside the existing seven, which must give the same results:
   - Same file, different variable (`order` instead of `user`) → **not** suppressed.
   - Another `.ts` file, same template with a different identifier (`token` / `validate`) → **not** suppressed.
   - Another `.ts` file, same identifiers (`user` in `getProfile`) and the same text → suppressed.
   - A rejection with no code spans matches only in the same file.
5. **"Fixed" un-reject.** A reply containing "not fixed" or "isn't fixed" doesn't count as acceptance. Add a fixture.
6. **Noise.** When nothing was suppressed, don't add the body line `0 findings suppressed…` or the `suppressed: []` footer field. The existing byte-identical test for `learn: 'off'` must still pass.

Push to `codex/learn-v1` (no force-push) and keep #49 as a draft. End with: the new head sha, one line per commit, test counts, and the full fixture table (case → expected → got).
