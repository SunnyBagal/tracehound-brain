# 071 report: Nemotron 3 Ultra on dev via the NVIDIA API (report only)

**Step 0:** passed. Main fast-forwarded to 36de717 (merge of #62); `## 070` count 1, `## 071` count 0. `pnpm test` and typecheck green. NVIDIA_API_KEY set. GIT_* check printed nothing. Baseline records present (3 Super run 1 under bench scores/; 2r, 4, 5, 6, 7r and 8 under dev-repeat).

**Plan:** commit cb58934, pushed 2026-10-24T06:19:48Z. Probe sent 06:19:54Z.

**Probe:** HTTP 200, finish stop, 23 prompt / 16 completion tokens, reasoning tokens not reported in usage (53 chars of reasoning_content), 1.3 s.
**Smoke** (sentry/0, grafana/0, keycloak/0): 1 of 3 PRs with a failed chunk (keycloak/0 diff#0, a 300 s client timeout, not HTTP); 0 4xx, 0 429, 0 truncations; kept 6 / 5 / 1. No stop rule fired.

**Run 10** (complete, 06:37Z → 09:51Z): 0 PRs with failed chunks, so run 11 was allowed. **Run 11** was launched unattended at 09:52Z by a detached chain script (`runs/071-chain.sh`; it writes `runs/071-report-final.txt` when done). My session budget ran out before run 11 finished, so the report uses the fallback: **run 10 alone**.

| row | judge | 3 Super mean (5) F | 3.5 mean (2) F | Ultra run 10 P / R / F |
|---|---|---|---|---|
| pool-verify-v1 | v1 | 42.3 | 50.4 | 34.8 / 54.1 / 42.3 |
| pool-verify-v1 | x1 | 51.3 | 55.1 | 45.9 / 67.6 / 54.6 |
| pool-raw | v1 | 38.2 | 53.9 | 31.2 / 64.9 / 42.1 |
| pool-raw | x1 | 47.0 | 54.9 | 39.0 / 77.0 / 51.8 |

Baselines recomputed: 42.28, 50.36, 38.16, 53.87 (all within 0.1).
Bootstrap (seed 71, 10,000 resamples), Ultra − 3.5 / Ultra − 3 Super F: pool-verify-v1 v1 −8.0 [−18.3, +2.7] / +0.0 [−8.7, +8.9]; x1 −0.5 [−11.2, +10.3] / +3.4 [−5.1, +11.5]; pool-raw v1 **−11.8 [−19.6, −4.0]** / +3.9 [−3.1, +11.5]; x1 −3.1 [−14.0, +7.7] / +4.8 [−3.2, +12.6]. Only pool-raw v1 Ultra − 3.5 excludes 0 (Ultra is worse there).

**Ops:** run 10 had 0 failed chunks, 0 VERIFY_FAILED, 0 calls at max_tokens, 0 HTTP 429, 306 requests and 3 timeouts (2 PR reruns, none still failing). Wall time 3 h 14 min; median 233 s per PR (3 Super 86–108 s, 3.5 121/130 s). Output tokens per PR: median 15,536 (3 Super 20,025, 3.5 50,064).
**Spend:** Nebius $0.1011 for run 10's judges, plus run 11's judges (pending, capped at $3). Ultra: 2,491,296 in / 688,589 out tokens, $0.

**PR:** #63 (draft), head 6bd72e1. CI result not checked by me (budget).
