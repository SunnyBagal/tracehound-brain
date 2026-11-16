# bunny 076o (raw, finish)

Finished 16:12:21Z. Five side-track dev review folders scored with the real bench-judge-v1, one process after the other: 82 paid judge calls, 0 failed, 0 judge failures, $0.048 of the $0.25 cap (the two typed-facts folders reused cached verdicts for their 39 unchanged reviews).

- All four Sonnet-with-skill draws (two full-packet, two short-packet) caught 17 of 31 bug cases; the Nemotron pipeline row caught 12. Against it, each Sonnet draw has 7 or 8 cases only it caught and 2 or 3 only the pipeline caught (exact p 0.18 to 0.23 per draw; not significant on any single draw).
- A/B 1 (Sonnet only): full-packet arm mean 17.0, short-packet arm mean 17.0; union 19 each, 1 case unique to each arm (p = 1.0). The short packet meets "no loss of 4+ catches" and has fewer clean-case comments (mean 2 vs 3). The cost condition waits on model-free numbers; the plan's Haiku half was not run.
- Typed facts: the one case with a typed fact was not caught with the fact in either draw (0 of 2), as without it.
- Proxy judge vs real judge on the two draw-1 folders: agreement 25 of 31 and 28 of 31 bug cases; 9 disagreements, 7 of them where the real judge counted a catch and the proxy did not.

Dev only; held-out untouched. Report in the testset run/out.
