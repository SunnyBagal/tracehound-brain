# Codex object-fetch stage one and source deviation (raw)

Stage one complete, identities pass; no freeze GO. All four clones have zero missing blobs. Across 29 cases, 58/58 base/case-head commits and root trees match the frozen identities, plus 29/29 original heads and trees. Refs, HEAD, index and persistent config are unchanged; temporary upstream remotes are removed.

Source deviation: the first worker added a URL instead of replacing the existing first remote URL, so plain Git reached two forks. It made 225 and 22,016 requested missing blob objects readable before the worker was interrupted. The other two clones were not contacted. The earlier upstream-success claim was retracted promptly; the deviation report preserves the failure. No web/API/PR refs, review-comment/result reads, push or fork writes occurred.

The revised authorized worker fetched only remaining objects through verified single-URL public-upstream remotes: 48,020 requested blobs, 95 fetches and 95 effective-URL proofs. Source deviation objects remain separately counted. One local missing-import setup error stopped before any network call and its record is preserved.

No stage two and no freeze GO until both Grok's independent identity GO and Sunny's ruling on retaining the fork-fetched objects. New scheduled start is 2026-11-19T04:30:00Z; the scheduler plan/list is not updated until the gated stage two. No post-materialization runner dry run or golden rebuild has run. Parent task stays incomplete.

Counts and method only; no case identities, individual case dates, object IDs or case text. $0, no provider/model call, P1 write, checkout, history rewrite, commit or merge by codex. Owner STATE/LOG handoff supplied. This raw note needs independent review before push.
