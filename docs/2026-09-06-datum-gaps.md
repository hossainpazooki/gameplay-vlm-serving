# Where TRAVERSE's 2026-09-05 design stands against DATUM

DATUM is the gate discipline restated in `docs/discipline.md`; rule numbers
below refer to that file.

2026-09-06. Moved here from DATUM v1, where it sat inside the governing
text; under DATUM rule 1 a per-repo report belongs in the repo it reports
on. Verified against `2026-09-05-design.md`, not against any code (none
exists). This list is authored, not generated; it becomes the seed of the
rule-by-rule table in this repo's README DATUM section once the pack is
vendored.

- Carries rules 2, 3 (the statement), 5, 7, 9, 11 as written.
- Rule 2, outcome form: the design uses `UNEVALUABLE:quota-not-granted`;
  under DATUM v2 that is `result: UNEVALUABLE` plus a separate
  `unevaluable_reason` field.
- Rule 3 mechanics: missing. No cell field, no planted block, PASS/FAIL
  instead of GREEN/RED, "a twin fixture that must fail" rather than "fails
  for exactly the planted reason".
- Rule 4: partially. Gates have twin fixtures; the status builder's own
  negative controls are not named.
- Rule 5: the design says STATUS.md is generated, never hand-written; the
  DATUM house rule is a hand-written dated record plus a generated block
  between markers in the same file.
- Rule 6: partially. git_sha present; no worktree flag, no content hash
  basis (manifest shas serve as content identity, basis unstated). Field
  names to be `gate_sha` / `gate_worktree`.
- Rule 8: the binding file and recomputing checker are not named.
- Rule 10: floors.json exists; provenance of floors unstated. Whether
  floors are set from a prior public benchmark or from the first run's own
  numbers, which would make the first run's PASS circular, is this repo's
  open decision.
- Rule 12: no statement; adopt MERIDIAN's practice.
- Runtime twins (tc throttle, cgroup io.max) are Linux-only and need the
  rented box, so they are not CI-reproducible; the siblings' twins all run
  in CI. Day 3's fresh-clone CPU reproduction covers fixture twins only.
