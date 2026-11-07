# leeway-lbi-ledger

Allocation ledger for the Leeway Build Identity (LBI). This repository holds no source:
its only content is git refs, each one an orphan commit whose tree carries one
`allocation.json`.

- `refs/lbi/seq/<N>`: build attempt with global sequence `N` (revision 1).
- `refs/lbi/rev/<N>/<R>`: servicing re-spin `R` (R >= 2) of sequence `N`.
- `refs/lbi/result/<N>`: optional post-build outcome written by the build runner
  (attempt id, manifest id, artifact hashes; WS-C provenance).

Allocation is a compare-and-swap on ref creation: a client reads the current maximum,
pushes `refs/lbi/seq/<max+1>` with `--force-with-lease=refs/lbi/seq/<max+1>:` (the ref
must not exist) and retries on rejection. A number that was allocated is never reused,
whether or not the build succeeded. Refs under `refs/lbi/` must never be deleted or
rewritten.

Specification: `LiberNT-LDW` branch `leeway/lbi-wsb-generator`, `sdk/tools/lbi/`
(allocator `lbi_alloc.py`, grammar `lbi_format.py`); hive document `WS-B-LBI-SPEC.md`.
