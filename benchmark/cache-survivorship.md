# Cache survivorship: reliability tests bypass the cache

**Standing rule (decision 073a, 2026-11-08; top of [docs/decisions.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md)):** a test of failure rates, retries or parsing draws every request under test fresh, so the response cache is neither read nor written for them. [docs/budget.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/budget.md) rule 2 repeats it: replay the cache unless reliability is the thing under test.

## Why
The client caches every **non-empty** HTTP 200 reply. A failed or empty reply is never cached. So a corpus replayed from the cache holds only the survivors, and it under-counts the very failures a reliability test is meant to measure.

## The case that set the rule (decision 073)
- **The setup:** 073's arms replayed the cache, and 83 of 96 first-attempt request keys were already in it.
- **The gap:** its baseline lost **1** chunk, while the original runs 7r and 8 had lost **8 each**. On our side the requests had not changed: same model, endpoint, prompts, `max_tokens` and request-building code.
- **What explains most of it:** the 54 stored first replies exclude every empty reply. Of 073's 10 empty first replies, 5 got their retry served from an October reply that had succeeded.
- **What is not shown:** a provider change (too few fresh draws to compare rates).
- **The consequence:** B1's "pass" was relabelled "harmless, benefit unproven" ([D073](../decisions/D073-recovery-ab.md)).

## Since then
- 074 drew its hard-chunk set fresh, 3 times each, with the cache bypassed. 26 of 42 first replies came back empty ([D074](../decisions/D074-hard-chunks.md)).
- 075, a quality test rather than a reliability test, read and wrote the cache as rule 2 allows ([D075](../decisions/D075-verify-a-vs-e.md)).

Related: [empty replies](../project/empty-replies.md) · [lessons](../lessons.md) (2026-11-08)
