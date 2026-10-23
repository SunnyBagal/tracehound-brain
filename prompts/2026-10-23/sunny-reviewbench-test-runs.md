# Sunny, by hand: ReviewBench portal configs + test runs (P008, amendment 5)

Written by chat, 23 Oct 17:55 IST. Image for every candidate:
`ghcr.io/sunnybagal/tracehound-reviewbench@sha256:9b9a5127317be79f4bc9f7e771b84b441d887f5cdd52e38e934b8657ceb84508`

Smokes on that image (23 Oct, all green, ~$0.32 total):

| run | pipeline / model | idx 0 | idx 4 |
|---|---|---|---|
| #10 | pool-raw / 3.5 | ok, 1 finding, $0.1148, 150 s | ok, 1, $0.0102, 13 s |
| #11 | pool-verify-v1 / 3.5 | **ok partial=1/2**, 0, $0.0926, 171 s | ok, 1, $0.0097, 15 s |
| — | pool-raw-x2 / 3 Super | ok, 2, $0.0848, 308 s | ok, 1, $0.0086, 27 s |

idx 0 on 3.5 was red under all-or-nothing (21 Oct). #11 shows the partial rule working: one of two reviewer chunks failed, and the PR still returned.

## A. Add configs 1–5
Portal → reviewer **TraceHound** → **+ Add a configuration** (never "Register a reviewer"). For each: image = the digest above; delete the pre-filled label keys; add exactly two labels; save; store credentials `NEBIUS_API_KEY` = the `reviewbench` key.

| candidate | `pipeline=` | `model=` |
|---|---|---|
| 1 | `pool-verify-v1` | `nvidia/nemotron-3-super-120b-a12b` |
| 2 | `pool-raw` | `nvidia/nemotron-3-super-120b-a12b` |
| 3 | `pool-raw-x2` | `nvidia/nemotron-3-super-120b-a12b` |
| 4 | `pool-verify-v1` | `nvidia/Nemotron-3.5-Super-VL` |
| 5 | `pool-raw` | `nvidia/Nemotron-3.5-Super-VL` |

In the dropdown each new config shows `9b9a5127…`. **The two `ff22b972…` entries are never started.** Delete them if the portal allows.

**→ send** a screenshot of the opened dropdown.

## B. Test runs, in this order
Each run happens **once** (P008: one test run per candidate). Select the config, check the digest in the dropdown says `9b9a5127`, then **Start test run**.

1. **Candidate 2**: pool-raw, 3 Super
2. **Candidate 5**: pool-raw, 3.5
3. **Candidate 1**: pool-verify-v1, 3 Super
4. **Candidate 4**: pool-verify-v1, 3.5
5. **Candidate 3**: pool-raw-x2, 3 Super
6. **Candidate 6**: pool-raw-missed. Chat tells you which model once 2 and 5 are done (the higher grounded F1; a tie within 2.0 goes to 3 Super). Add its config then.

2 and 5 go first because candidate 6's model depends on them.

Start one, and see how long it takes. If the portal lets several run at once and the first one finishes cleanly, start the rest together; otherwise one after another.

**Stop and send a screenshot** if any of these happens:
- the portal shows a limit on the number of test runs;
- a run ends failed or errored;
- a run shows any PR failing.

Don't re-run anything.

## C. What to send, per run
A screenshot of the run's **summary**: status, grounded P/R/F1, augmented numbers if shown, cost and duration.

Per-PR detail is read **only** to check the run worked (status, finding counts), per P008 amendment 3. Nothing gets tuned on it. Don't open or paste golden comments.

## Selection (pre-registered, P008; chat applies it)
- The highest grounded F1 among 1–5. If two are within 2.0 points, take the cheaper, in the order pool-verify-v1, pool-raw, pool-raw-x2. Same-price models count as equal cost, and a tie goes to 3 Super.
- Candidate 6 wins only if it beats the best of 1–5 by **≥ 3.0**.
- Then **one** final run (219 × 3) with the chosen config. 3.5 pool-verify ≈ $41, 3 Super pool-verify ≈ $21.

Budget: the console has $70.69 after the top-up. All six test runs ≈ $8. That leaves enough for the 3.5 final run plus the Cal.com v2 run (~$4), but not much more.
