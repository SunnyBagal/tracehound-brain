# The 300 s transport cutoff (product bug, fixed 2026-11-08)

## What it was
The model client gives each request a 600 s timeout, but it sent requests through Node's built-in `fetch` (undici). undici has its own `headersTimeout` of 300 s, so any reply slower than 300 s failed at exactly 300 s with `fetch failed`.

## How it was found
- **The void run:** decision 073's first run, on 2026-11-07, saw live 3.5 calls fail at exactly 300 s after sending. That was 4 of 11 calls, about 5 minutes apart in the ledger, because the long-reasoning replies took longer than 300 s.
- **Cost:** the run was voided, and its 11 calls cost $0.34415 ([D073](../decisions/D073-recovery-ab.md)).
- **Why that mattered:** a failed request is never stored, so it would have gone out live again in every arm. Every such pull would have been unpaired, and the result inconclusive.

## The product had it too
The Action runs the analyzer CLIs (`review`, `review-ctx`, `verify-ctx`). They set `timeoutMs: 600_000` but used `globalThis.fetch`, so the GitHub Action had the same hidden 300 s limit.

## The fix
- **`longFetch`:** requests now go through `longFetch` (node:http/https, no limit of its own) in `packages/analyzer/src/llm/transport.ts`. Only the client's 600 s abort signal ends a request.
- **Unchanged:** request bytes, cache keys and ledger entries.
- **Test:** `packages/analyzer/test/llm-transport.test.ts`.
- **Merged:** on 2026-11-08 with decision 073 (`fd36552`).
- **Exception:** the opt-in NVIDIA route ([D071](../decisions/D071-ultra-dev.md)) keeps its deliberate 300 s timeout, now enforced by its own signal.

## What it did not explain
October's chunk losses were invalid JSON, not timeouts: runs 7r/8/9 show 0 fetch errors ([empty replies](empty-replies.md)). The cutoff became frequent only in 073's run.

Source: [decisions.md 073a, "The product had the same cutoff"](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#073--pre-registered-dev-ab-of-the-two-generation-recovery-switches-2026-11-08).
