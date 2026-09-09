# gameplay-vlm-serving

Raw gameplay video to training-ready tensors to a served vision-language
model, with every number behind a gate that has been seen to fail.

**State: design only.** No pipeline code, gates, or infrastructure exist in
this repository yet. The design is `docs/2026-09-05-design.md`; the gate
discipline every document here refers to is restated in
`docs/discipline.md`; dated state and open items are in `docs/handoff/`.

## Scope walls

- Claims are rows. Nothing in this README carries a count or a status. Until
  a STATUS.md exists, nothing is claimable.
- A gate is credited only when its live cell is GREEN and its twin, the same
  input with one planted defect, is RED for exactly the planted reason.
- Floors are authored numbers; each names its source. A run on an instance
  type with no floor is UNEVALUABLE, not FAIL. A floor derived from a run's
  own numbers is descriptive, never a claim.
- Every stage reads one manifest and writes one manifest and never touches
  another stage's files.
- Rows from runs that cannot be regenerated are committed and hash-bound;
  the hand-copy seam is otherwise unverified and is said to be.
- Corrections to dated records are appended in place, never erased.
- Nothing about multi-region, nothing about scale beyond the measured
  corpus, nothing about training. No production claim.

## Where things are

`docs/2026-09-05-design.md` the design and its dated amendments.
`docs/discipline.md` the twelve rules and the verdict row, self-contained.
`docs/2026-09-06-datum-gaps.md` where the design stands against those rules.
`docs/handoff/` dated briefs, newest first in `HANDOFF.md`.
`docs/learnings/` one verified fact per dated file, indexed in `LEARNINGS.md`.
