# bunny overnight · task 2 · flaky-061a-restart-test

## Started (2026-11-14T23:41:54Z)
The 061a restart test in packages/analyzer/test/test-set-run.test.ts failed once in a full analyzer run on 14 Nov, then passed alone and in a rerun. Plan: measure the rate alone and in the full suite (20+ each), find the cause, fix it on a test-only branch if possible. $0.

## Finished (2026-11-15T00:13:06Z)
Done. Cause: inventedBench() spawned git 11 times per call; the 061a refusal test builds 3 benches (~0.6 s alone) and ran past vitest's 5 s timeout under the full workspace's parallel load. Baseline on main 4fce41e: 4 of 20 full runs failed (all "Test timed out in 5000ms" in test-set-run.test.ts), 0 of 25 alone. Fix (test code only): build the bench once per file and cpSync a fresh copy per call. With the fix: 0 of 20 full runs, 0 of 25 alone; the test 103-124 ms. PR #78 (open, not merged), branch flaky-061a-restart-test, head 9ac699b. $0.
