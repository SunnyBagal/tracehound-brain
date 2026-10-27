# seed-01b: stop report (after the 3-PR gate)

The 3-PR gate passed (live config confirmed). Stopped anyway for two blockers. Ruling since received: Devin access is being removed; Recall continues now, Linea waits for review-freeze-2.1.

## PRs opened: 3 of 30 (Recall 2, Linea 1)

| PR | Run | Reviewer step | TraceHound output |
|---|---|---|---|
| https://github.com/SunnyBagal/recall-seeded/pull/1 | 40556700220 green | 13.5 s | no review posted ("reviewer returned no findings"); diff 2 files, +4/-3 |
| https://github.com/SunnyBagal/linea-seeded/pull/1 | 40564762809 red, rerun once, red | skipped | nothing |
| https://github.com/SunnyBagal/recall-seeded/pull/2 | 40564970734 green | 182.7 s | review, header names nvidia/Nemotron-3.5-Super-VL, 0 findings (pool 1, kept 0) |

## Blocker 1: Action install fails on linea-seeded
- Both attempts fail in the Action's pnpm/action-setup step: "Multiple versions of pnpm specified". Action pins pnpm 11.20.0; Linea's root package.json pins pnpm@9.0.0. Fails before any model call.
- Recall has no root package.json and is unaffected.

## Blocker 2: Devin Review auto-reviewed the seeded PRs
- devin-ai-integration[bot] posted 1 review + 1 inline comment on each of the 3 PRs, about 1.5 min after opening (6 items total). Installed account-wide; not visible from the repo (0 webhooks).
- Not scored (scorer counts github-actions[bot] only). Comments left untouched.

## State
- Script stopped; no PRs commented on, closed or edited.
- Nebius spend: not shown in the run logs.
