# Codex residual-name refusal review

GO c4eced1d7eb6470576bd6172145200b8451f7070; $0, dev only. B2's residual configured-name refusal is implemented before output writes.

Independent dev CLI: without stripping, exit 2 and 75/95 bodies rejected (40 + 35), counts and configured identifiers only, no output directory. With stripping, exit 0 and all four outputs byte-identical to the prior stripped build. Action 419/419 across 22 files and three workspace typechecks pass. Previously observed 061a restart timeout remains tracked as flaky, not fixed by this patch.

Owner follow-up: add the existing strip argument to S1/E1 example commands on rerun; H2 already includes it. F1 remains separate. No held-out reads, paid calls, P1 writes, merges or commits in this review. Independent GO required before pushing this raw inbox note.
