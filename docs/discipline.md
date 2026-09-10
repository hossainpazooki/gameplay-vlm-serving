# The gate discipline this repository follows

Other documents in this repository cite "DATUM" and "DATUM rule N". DATUM is
a governing text the author shares across three private data-platform
repositories; it is not published. This file restates the parts of it that
bind this repository, so a reader needs nothing outside this tree to follow
any reference. Where a cited document says "DATUM v1" or "v2", v2 is the one
in force; the differences that matter here are listed at the end.

## One sentence

A data platform is credited by what its gates have shown, never by what its
author says, and a gate counts only after it has been seen to fail.

## The twelve rules

Each rule depends on the one before it.

1. **The record is a row, not a sentence.** Every claim resolves to a
   machine-checkable verdict row carrying the identity of the code that
   produced it and the content it ran over. Prose points at rows and never
   carries a count or a status a row does not carry first.
2. **A gate has three outcomes.** GREEN, RED, and UNEVALUABLE with a stated
   reason in its own field. Unevaluable is neither a soft pass nor a soft
   fail; it halts whatever depends on it. A zero evaluated denominator forces
   UNEVALUABLE.
3. **A gate that has never gone red is not a gate.** Every gate ships with a
   twin: the same input with exactly one planted defect. The gate is credited
   only when the live input is GREEN and the twin is RED for exactly the
   planted reason, compared key for key over the union of both key sets. A
   twin that comes back GREEN, or RED for a different reason, is a broken
   gate and the checker refuses to derive status rather than render a wrong
   table. A property with several twins needs every one of them red as
   planted.
4. **The checker has its own negative controls, and says what it cannot
   check.** Fixture rows that must pass and fixture rows that must each fail
   for a named reason run in CI before anything is built. A refusal nobody
   planted is itself a failure. "Governed" is never read as "machine-checked"
   for rules the checker cannot reach.
5. **Status is derived, never authored.** The claimability table is computed
   from rows at build time; the literals CLAIMABLE, PARTIAL, UNCLAIMED found
   inside any row fail the build; the committed rendering is compared against
   a fresh one in CI. STATUS.md holds a hand-written dated narrative plus a
   generated block between markers, both checked.
6. **Identity is content plus code plus worktree.** A row carries a content
   hash with its basis written beside it, the emitter's commit in `gate_sha`,
   and `gate_worktree` as clean or dirty. A dirty-tree row is evidence about
   code nobody can check out and is refused. Hashes are over LF-normalised
   bytes where the consumer is newline-insensitive.
7. **Nothing is a claim until the row sits on a pushed SHA.** A row in a
   working tree is a draft. For runs CI can regenerate, the claim is the CI
   run green on a pushed commit. Gates that depend on other gates are
   UNEVALUABLE until the rows they depend on are pushed.
8. **Rows that cannot be regenerated cross a hand-copy seam, and the seam is
   guarded.** Rows from a rented GPU or a box that no longer exists are
   committed, bound file by file to a sha256 in a source file the checker
   recomputes; any unlisted, missing, or altered file fails. Rows CI can
   regenerate are build output and are not committed.
9. **Effects are probed, never read from the action's own report.** A
   destroy that says it destroyed is a claim about the action. Teardown is
   verified by an inventory query by tag that must return empty, run twice;
   Terraform state is an index, not a decision. A probe that would answer
   the same either way earns nothing.
10. **Thresholds are declared with their provenance.** A floor or tolerance
    is an authored number and therefore a status in disguise. Each lives in
    one file, keyed by the condition it applies to, with its source written
    beside it. A floor derived from a run's own numbers makes that run's pass
    circular and is labelled descriptive, not a claim.
11. **Scope walls stand before code.** The README names what the repository
    does not do and will not claim; a claim ceiling lists what may be claimed
    after the last planned run. Precise verbs: we validate, we use as a
    baseline, we propose.
12. **Corrections are appended in place, dated, never erased.** This governs
    records: rows, STATUS entries, learnings, handoffs. A governing text is
    revised instead, with a section naming what changed and why.

## The verdict row

Field names live in a JSON Schema the shared pack will carry (schema id
`datum/gate-verdict/1`). The invariants:

- it names `kind` (GATE_VERDICT), `schema`, `surface`, `lane`, and `cell`
  (live or twin);
- a twin carries `planted` with `mutation`, `mutated_rows`, and
  `expected_violations`; a live row carries none;
- `result` is exactly GREEN, RED, or UNEVALUABLE, with `unevaluable_reason`
  required in the third case and forbidden otherwise;
- every entry in `checks` has a matching `evaluated` denominator;
- `content_hash`, `content_hash_basis`, `gate_sha`, `gate_worktree`,
  `ran_at`, and `runner` bind it to content, code, and tree;
- repo-specific measurements go under `metrics` and never displace a field
  above.

## What this repository owes under the discipline

Not yet built, as of this file's date. When built: a checker with its own
controls; fixture twins for every gate, runtime twins for the fetch and
decode gates on the rented instance; `gates/floors.json` with a source per
floor; a STATUS.md with a generated block; committed hash-bound rows under
`runs/<run_id>/`; a vendored copy of the shared conformance pack pinned by
content hash once that pack exists; and a README section reporting, rule by
rule, which rules the pack checks and which are only authored.

## v1 versus v2, for readers of the dated documents

The design document's 2026-09-06 amendments and the first implementation
plan were written against v1. v2 changed the field names to `gate_sha`,
`gate_worktree`, and `unevaluable_reason`, added the `schema` field, chose
`surface` over `stage`, made STATUS.md a hand-written record plus a
generated block rather than a fully generated file, and stated that a
governing text is revised rather than amended in place. The design was
revised to v2 on 2026-09-09 (see its Changes section) and the plan with it;
the 2026-09-07 handoff brief and the learnings entries that name the v1
gap are records and stand as written.
