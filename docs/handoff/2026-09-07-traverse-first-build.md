# Handoff — traverse first build: repo seeded, design amended, Plan 1 written, DATUM v2 drift found

2026-09-07 (session ran 2026-09-06 local). Newest commit this brief describes: **none**. The traverse repo has zero commits; everything below is an untracked working tree. Pick-up measures drift from the file hashes in "Current state", not from a SHA.

## Current state

- **built** Repo directory `~/dev/traverse`, `git init -b main`, no commits. `.gitignore` present; `docs/plans/` excluded locally via `.git/info/exclude`.
  re-verify: `cd ~/dev/traverse && git rev-parse HEAD 2>&1 | head -1 && git status --short && tail -1 .git/info/exclude` expects `fatal: ambiguous argument 'HEAD'`, two `??` lines (`.gitignore`, `docs/`), and `docs/plans/`.
- **built** Design doc `docs/2026-09-05-design.md`, the operator's original plus three dated "Amended 2026-09-06" paragraphs (Shape, Verdict schema, Gates), a header line, and an "Amendments" section aligning it to DATUM **v1**. Original text untouched.
  re-verify: `grep -c "Amended 2026-09-06" docs/2026-09-05-design.md` expects `3` (the header line reads "Amended:" with a colon and does not match); `grep -c "^## Amendments" docs/2026-09-05-design.md` expects `1`.
- **built** Plan 1 `docs/plans/2026-09-06-plan1-core-pipeline-and-ledger.md`, 13 tasks, test-first, full code, operator checkpoints. Local-only (excluded). Written against DATUM **v1**.
  re-verify: `grep -c "^### Task" docs/plans/2026-09-06-plan1-core-pipeline-and-ledger.md` expects `13`; `grep -c traverse_sha` on the same file expects `8` (the v1 field name, see Open 1).
- **built** `docs/2026-09-06-datum-gaps.md`, written by another session (mtime 2026-09-06 23:42 -0400), not by this one. Rule-by-rule gap list against v2; seed of the future README DATUM table.
  re-verify: `grep -c "gate_sha\|unevaluable_reason" docs/2026-09-06-datum-gaps.md` expects `2`.
- **built** Learnings ledger: five entries plus `docs/learnings/LEARNINGS.md`, one refuted-assumption among them.
  re-verify: `ls docs/learnings | wc -l` expects `6`; every entry's `re-verify:` line is read-only and runnable from any shell with `$HOME/dev` siblings present.
- **built** Environment bet for Plan 1 Task 7 verified: PyAV 18.1.0 abi3 wheel on Python 3.14 encodes libx264. Scratch venv at the session scratchpad, not in the repo.
  re-verify: `python -m pip index versions av | head -1` expects `av (18.1.0)` or newer.
- **in-progress** Nothing. No code exists in traverse.
- **planned** Plan 1 execution (all 13 tasks). Plan 2 (VM lane: Terraform, S3 fetch, vLLM sweep and its two gates, runtime twins, teardown inventory, committed hash-bound runs). Plan 3 (EKS lane). Neither plan 2 nor 3 is written.
- **planned** Re-alignment of spec and Plan 1 to DATUM v2 (Open 1). Vendoring the DATUM conformance pack, which itself does not exist yet.

## Locked decisions

- **DATUM v2 at `~/dev/datum` `91be859` governs traverse**, by operator ruling recorded in that file's header. v1 at `~/dev/briefs/DATUM.md` is superseded and local-only. Reason: one governing text for baseline, traverse, meridian, with an executable pack. Pick-up: `git -C ~/dev/datum log --oneline -1` and check the header still says governing.
- **Three plans, not one.** Plan 1 is the CI-regenerable core; plan 2 the VM lane; plan 3 EKS. Reason: each produces gated software on its own, and rented-GPU rows cross the hand-copy seam (DATUM rule 8) while synthetic-clip rows do not.
- **Row shape is the shared schema, not the design's flat PASS/FAIL row.** Reason: the flat row cannot express "twin RED for exactly the planted reason" (DATUM rule 3). The exact field names are v2's (see Open 1); the decision to use the shared shape is not up for relitigation.
- **Synthetic clips are committed and pinned by SHA256SUMS, not regenerated in CI.** Reason: encoders are not byte-deterministic across FFmpeg builds; a freshness diff would flake for a reason that looks like fixture staleness. The generator is documentation of how they were made.
- **CI-regenerable rows are build output (`gates/out/`, gitignored); unrepeatable rows are committed under `runs/<run_id>/verdicts/` with a hash-binding `SOURCE.md`.** Reason: DATUM rule 8 both cases; matches MERIDIAN for the first and BASELINE for the second.
- **Floors carry a `source`; the g5 floor is null until a cited prior figure exists, and is never set from the first run's own numbers.** Reason: a self-derived floor makes the first PASS circular (DATUM rule 10). datum's design lists floor provenance as traverse's open item, so the null-until-cited rule is this repo's answer, not DATUM's.
- **Pipeline stages, gates, and the status builder in Python.** Original reason "one CI toolchain" **no longer holds**: v2 makes the vendored pack Node 20, so CI needs both. Keep Python for the repo's own code (PyAV, numpy, pytest); do not relitigate to Node for the sake of one runtime.
- **Local codename `traverse`.** Directory name fixed; GitHub slug not locked (Open 4).

## Reuse map

- `~/dev/parallax/gate/verdict.py`: the verdict emitter Plan 1 Task 3 ports (`verdict_row`, `write_verdict`, `_git_state`). Needs v2 names.
- `~/dev/baseline/scripts/check-ledger.mjs`, `scripts/lib/ledger.mjs`, `scripts/test-ledger.mjs`: checker structure, union-of-keys plant match, positive and negative controls. Plan 1 Task 5 mirrors them in Python; once the datum pack exists, vendor it instead of maintaining a parallel checker for rules 1, 3, 5, 6.
- `~/dev/baseline/ledger/verdicts/*twin*.json`: a real twin row to copy the `planted` block shape from.
- `~/dev/meridian/gates/claimability.py`: multi-twin crediting grouped by `planted.mutation`; `gates/run.sh` and `.github/workflows/gates.yml`: runner and CI shape Plan 1 Tasks 11 and 13 copy.
- `~/dev/datum/docs/2026-09-06-datum-design.md` §2 (schema v1 table), §3 (crediting), §5 (what a governed repo carries: `vendor/datum/{check.mjs,test.mjs,fixtures,reasons.md,PIN}`, CI order, README DATUM section).
- `docs/2026-09-06-datum-gaps.md` in this repo: the authored rule-by-rule table to seed the README DATUM section.
- Plan 1 code blocks: `traverse/canon.py`, `manifest.py`, `stages/{fetch,decode,pack}.py`, `gates/{checks,twins}.py` are v2-neutral and reusable as written; `verdict.py`, `gates/ledger.py`, `gates/check_ledger.py`, `scripts/build_status.py`, and the six fixture rows under `fixtures/rows/` are the parts that change under Open 1.

## Invariants

- **Every stage reads one manifest and writes one manifest and never touches another stage's files.** Plan 1 tests assert this by directory listing and mtime (`test_fetch_never_writes_outside_its_own_dir`, `test_pack_touches_only_its_own_dir`). Break it and the Kubernetes lane in plan 3 has no contract.
- **A twin is credited only when RED for exactly the planted reason, over the union of both key sets; a GREEN twin or a wrong-reason RED is a refusal, not a status.** Break it and the claimability table renders a wrong row instead of stopping.
- **No status literal (CLAIMABLE, PARTIAL, UNCLAIMED) inside any row.** The checker fails on one. Break it and status becomes authored.
- **A row from a dirty worktree is invalid.** The driver run in `gates/run.sh` uses the real repo state, so the operator must commit before rows count; `pytest` runs use temp repos and are unaffected. Break it and rows describe code nobody can check out.
- **UNEVALUABLE propagates: a twin over an unevaluable live cell is unevaluable with the same reason; a zero evaluated denominator forces UNEVALUABLE (v2).** Break it and a gate that could not run reads as a pass or a fail.
- **All JSON written with sorted keys, LF, trailing newline; hashes over LF-normalised bytes; `.gitattributes` forces LF.** Break it and content hashes and SOURCE.md pins false-alarm on checkout.
- **Git history is the operator's.** No `git commit` or `git push` by an agent in this repo; plan checkpoints print the command. `~/dev/briefs/` is local-only and never referenced from committed text as a source.
- **Records are corrected in place with a date; governing texts are revised with a changes section (v2 rule 12).** The design doc is a governing text; see Open 1 item 5 for the consequence.

## Open / next

1. **Re-align the spec and Plan 1 to DATUM v2** (first thing; do it before any code, per datum design §6 step 4 "born conforming"). Exact deltas, all captured in learning `2026-09-07-spec-plan-follow-datum-v1`:
   - `traverse_sha`/`traverse_worktree` become `gate_sha`/`gate_worktree` (8 occurrences in the plan, 1 in the spec).
   - `reason` becomes `unevaluable_reason`, required iff UNEVALUABLE and forbidden otherwise.
   - Add `schema: "datum/gate-verdict/1"`; rename `stage` to `surface` (pattern `^[a-z0-9][a-z0-9-]*$`); `rows` and `metrics` optional; refuse additional properties.
   - STATUS.md becomes hand-written dated record plus a generated block between `<!-- datum:status:begin -->` / `<!-- datum:status:end -->`, with `--check` comparing only the block. Plan 1 Task 12 and the spec's "STATUS.md carries no corrections" line change.
   - The spec's four in-place amendments are the record form applied to a governing text. v2 rule 12 says revise the body and add a "Changes" section instead. Fold the amendments into the body, keep the Amendments section as the change log, and drop the header line that calls them in-place corrections.
   - Add to the plan: `evaluated[k] == 0` forces UNEVALUABLE in `GateReport.from_checks`; live with non-zero checks must be RED; a README DATUM section seeded from `docs/2026-09-06-datum-gaps.md` with rules 1, 3, 5, 6 marked "pack result" and the other eight "authored".
   - Blocker for the vendoring step only: the pack (`~/dev/datum/conformance/`, `schema/`) does not exist. Plan 1 can proceed with its own Python checker for now; vendoring becomes a Task 14 when the pack lands, and the Python checker's rules 1, 3, 5, 6 then defer to it.
2. **Confirm with the operator that `docs/2026-09-06-datum-gaps.md` is theirs** and stays as the seed of the README table. This session did not write it and did not edit it.
3. **Execute Plan 1** once item 1 is done: subagent-driven with review between tasks, or inline with checkpoints. Every checkpoint is an operator commit; the first `gates/run.sh` end-to-end needs a clean tree.
4. **Lock the GitHub slug.** Candidates: `gameplay-vlm-serving` (recommended: keeps the corpus hook for the target reader) or `vlm-serving-pipeline` (portable, loses the hook). Both free under `hossainpazooki` as of 2026-09-06. The slug is also the `repo` tag on every AWS resource in plan 2, so lock it before the first apply. Then `gh repo create hossainpazooki/<slug> --private --source=. --push`.
5. **Floor provenance for g5.xlarge** (DATUM rule 10, traverse's open item per datum design §9): find a citable gp3 or g5 S3-to-EBS figure, or leave null and report throughput as descriptive.
6. **Write Plan 2** (VM lane) after Plan 1's CI is green on a pushed SHA. It owns: `S3Source`, `serve` stage and its two gates, runtime twins (tc, cgroup io.max; Linux only), teardown inventory by tag, cost backfill, the `runs/<run_id>/SOURCE.md` template, and licence/revision fields becoming required for runners other than `local` and `ci`.
