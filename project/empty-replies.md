# Empty replies: why reviewer chunks fail on Nemotron 3.5

**Status (2026-11-09): a known limitation, not fixed.** The defaults are the append retry after an invalid reply plus the lenient parser. Reopen only with a new mechanism or a provider change, with its own budget line.

## The symptom
On 3.5 Super VL dev runs 7r and 8, 7 of 40 PRs per run lost a reviewer chunk and 3 of 40 ended with no findings ([D067](../decisions/D067-nemotron-35.md), [D068](../decisions/D068-partial-reviews.md)).

## The root cause, step by step
1. **It was not the transport.** 073a's failure audit counted terminal failed chunks in 3.5 runs 7r / 8 / 9: 8 / 8 / 4, all "not valid JSON", with 0 fetch or timeout errors ([decisions.md 073a](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#073--pre-registered-dev-ab-of-the-two-generation-recovery-switches-2026-11-08)).
2. **Most were empty, not malformed.** A lenient replay of those 20 failed chunks recovered 1. 14 of the 20 had an empty first reply, and 5 had a failing draw that was never stored. There were 0 truncated or malformed JSON cases, so a better parser can address only a small share.
3. **The reasoning uses the whole output budget.** In 073's records, all 35 empty replies ended `finish_reason: length` with reasoning tokens = completion tokens = 32,000. In 074's fresh draws, 26 of 42 first replies were empty in the same way ([D074](../decisions/D074-hard-chunks.md)).
4. **There is no budget knob.** Token Factory honours no reasoning-token budget. `reasoning_budget` is ignored ([D070](../decisions/D070-recall-v3.md)), and `reasoning_effort` is a level, not a token cap ([decision 074 (a)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#074--pre-registered-recovering-empty-replies-on-the-hard-chunk-set-2026-11-08)).

## What was tried
| try | result | note |
|---|---|---|
| lenient parser (073 B1) | default since 2026-11-08; "harmless, benefit unproven" | [D073](../decisions/D073-recovery-ab.md) |
| fresh retry with reasoning off (073 B2) | rejected: 7 of 10 fresh retries were invalid JSON | [D073](../decisions/D073-recovery-ab.md) |
| split the chunk in half (074 D) | 30 of 42 successes, under the 33 the gate needed | [D074](../decisions/D074-hard-chunks.md) |
| reasoning off + JSON schema (074 E) | won 074's gate (41 of 42), but its extra findings did not become kept TPs; dropped | [D074](../decisions/D074-hard-chunks.md), [D075](../decisions/D075-verify-a-vs-e.md) |

What the current append retry recovers: on the 26 empty-first units, 7 ended with a valid reply and a kept finding, and 15 with any valid reply.

## What is next
The 2026-11-10 planner proposes a third attempt (a plain redraw) after an empty reply, tested with the cache off ([docs/plan/2026-11-10-plan.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/plan/2026-11-10-plan.md) TL;DR item 5). It is not decided.

Related: [300 s cutoff](300s-transport-cutoff.md) (a different failure that looked similar) · [cache survivorship](../benchmark/cache-survivorship.md) (why a replay under-counts empty replies).
