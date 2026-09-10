# Handoff — full build plan written, dry-run verified, ready to execute

2026-09-10. Newest commit this brief describes: `c6bafff0487349dc065b85ab7f1e255e4c3384b8` on `main` of `hossainpazooki/gameplay-vlm-serving`. Measure drift from there, but note that the tree is **not clean**: four tracked files carry uncommitted edits and this brief and five learnings entries are untracked. The build plan itself is git-excluded and lives only on the operator's disk, so a fresh clone will not contain the largest artifact this brief describes.

## Current state

- **built** The repository exists, is public, and has two commits: `4506487` seeded the design and the ledgers, `c6bafff` added the README and a self-contained restatement of the gate discipline so that references to the private governing text resolve inside the tree.
  re-verify: `git -C ~/dev/traverse log --oneline -2` shows those two subjects and `git remote get-url origin` ends `gameplay-vlm-serving.git`.
- **built** `docs/plans/2026-09-09-full-build.md`: the whole build as 39 test-first tasks in four parts (Part 0 doc fixes, Part 1 core pipeline, Part 2 VM lane, Part 3 EKS lane), every task carrying complete code, exact commands, and expected output. It is local-only by the standing convention that superpowers plans stay out of history.
  re-verify: `grep -c '^### Task' ~/dev/traverse/docs/plans/2026-09-09-full-build.md` prints `39`; `md5sum` of that file prints `0eebdf88c496ca856e5a2eb69358bbff` unless it has been edited since, which any correction to it will do.
- **built** The plan's Python is verified by execution, not by reading. Every labelled code block was extracted into a scratch tree, every prose Modify instruction applied literally, and the suite run under Python 3.14 with PyAV 18.1.0: 100 of 100 pass. Eleven defects found on the way are fixed in the text and listed in the plan's own dry-run section.
  re-verify: `sed -n '/^## Dry run of this document/,/^Not exercised/p' ~/dev/traverse/docs/plans/2026-09-09-full-build.md` prints the record; the scratch tree that produced it does not survive the session, so this line checks the claim is recorded, not that it is re-runnable from the repository alone.
- **built** The plan's three Terraform modules pass `terraform fmt -check` and `terraform validate` offline, and its Kubernetes manifests parse as YAML after rendering.
  re-verify: extract the `infra/*` blocks and run `terraform validate` in each; the last run printed `Success! The configuration is valid.` for provision, vm, and eks.
- **built** The learnings ledger holds ten entries and its index; five were added today.
  re-verify: `ls ~/dev/traverse/docs/learnings | wc -l` prints `11` (ten entries plus the index), and every entry's `re-verify:` line is read-only.
- **in-progress, uncommitted** Four tracked files carry the second-revision re-alignment of the design to the governing text's current field names and status form: `README.md`, `docs/2026-09-05-design.md`, `docs/2026-09-06-datum-gaps.md`, `docs/discipline.md`. Part 0 of the plan edits three of these again and deletes the fourth, so committing them first is tidier but not required.
  re-verify: `git -C ~/dev/traverse status --short` lists those four as ` M`, alongside the two index files this handoff appended to and the six files it added.
- **not started** All code. There is no `traverse/`, `gates/`, `scripts/`, `tests/`, `infra/`, `k8s/`, `fixtures/`, `runs/`, `STATUS.md`, or CI workflow in the repository. Everything above describes a plan, not a build.
  re-verify: `git -C ~/dev/traverse ls-files | grep -c '^docs/\|^README\|^.gitignore'` equals the total file count from `git ls-files | wc -l`.
- **not started** Both AWS identities. Neither the `traverse-platform` nor the `traverse-provision` profile exists on this machine and the `default` credential no longer authenticates, so no part of the VM or EKS lane can begin.
  re-verify: `aws configure list-profiles` prints `default` and `kv-platform-admin` only.

## Locked decisions

- **Repository name `gameplay-vlm-serving`, local codename `traverse`.** Chosen so the slug names the artifact for the target reader while the family's one-word codename convention holds; already pushed, and the plan makes the same string the `repo` tag on every AWS resource and the teardown inventory's filter, so changing it after the first apply orphans resources from their own probe.
- **The public-repo wording rule.** The repository is public and the governing text it follows, DATUM, is private, so each document names that text exactly once and carries no path to it, no commit of it, no rule number from it, and no schema literal from it in prose; rules are referred to by description. Ruled by the operator on 2026-09-09 and applied to a sibling repository the same day. Part 0 exists to apply it here; records written before the ruling keep their wording and are superseded rather than edited.
- **Four parts, executed in order, doc fixes first.** Each part produces gated software on its own, and Part 0 runs first so no commit of code ever carries the pre-ruling wording.
- **Two AWS identities, and the Terraform split follows them.** `traverse-provision` creates IAM roles because the operational policy denies role creation; `traverse-platform` does everything else. `infra/provision` is applied by the first, `infra/vm` and `infra/eks` by the second, and the VM module reads its instance profile by name rather than creating it.
- **The shared verdict row, not the design's original flat row.** The flat row could not express "this twin is red for exactly the reason planted in it", which is the crediting rule the whole ledger rests on. Field names, the reason-in-its-own-field form, and the schema identifier are fixed in code and absent from prose.
- **Synthetic clips are committed and pinned by checksum, never regenerated in CI.** Encoders are not byte-deterministic across FFmpeg builds, so a freshness diff would fail for a reason that looks like fixture staleness and is not.
- **Rows CI can regenerate are build output; rows from rented instances are committed and hash-bound.** A GPU run cannot be replayed in CI, so its rows cross a hand-copy seam that a checksum file bounds and the checker recomputes.
- **Every floor carries its source, and the g5 fetch floor is a declared fraction of the provisioned volume throughput, not a measurement.** A floor set from the first run's own numbers makes that run's pass circular; the floor's source string says which kind of number it is.
- **Python for the repository's own code.** The original reason, one CI toolchain, no longer holds because the governing text's future conformance checker is Node; the decision stands on the pipeline's dependencies (PyAV, numpy) rather than on toolchain count.
- **Git history is the operator's.** Agents output commit commands and never run them. Two deviations in the plan are deliberate and recorded there: the sweep-completeness gate goes red on a missing cell so its twin can be red as planted, with the design's "a missing cell is unevaluable" carried by the dependent regression gate instead; and the cost-backfill surface has no twin by construction, so it derives as partial and the README says so.

## Reuse map

- `~/dev/parallax/gate/verdict.py` is the emitter the plan's `traverse/verdict.py` is ported from, including the git-state helper and the live-versus-twin argument checks.
- `~/dev/baseline/scripts/check-ledger.mjs` and `scripts/lib/ledger.mjs` are the checker's structure, including the union-of-keys plant comparison; `scripts/test-ledger.mjs` is the pattern for the checker's own negative controls.
- `~/dev/baseline/ledger/verdicts/*twin*.json` is a real twin row to copy the planted block's shape from.
- `~/dev/meridian/gates/claimability.py` is the multi-twin crediting rule; `gates/run.sh` and `.github/workflows/gates.yml` are the runner and CI shapes the plan's Tasks 1.11 and 1.13 follow.
- `docs/discipline.md` in this repository is the self-contained restatement of the rules; cite it rather than the private text.
- The plan is itself the reuse map for the build: every file the repository will contain appears in it as a complete code block, so nothing needs designing again.
- The scratch extractor that turned the plan into a running tree did not survive the session. It is about forty lines (walk the plan for `` `path`: `` followed by a fence, write each block, then apply the Modify instructions) and is worth rebuilding if the plan is edited substantially before execution.

## Invariants

- **Every stage reads one manifest and writes one manifest and never touches another stage's files.** Two tests assert it by directory listing and mtime. Break it and the Kubernetes lane loses the contract that lets one stage run alone in a pod.
- **A gate is credited only when its live cell is green and every twin is red for exactly the planted reason, compared key for key over the union of both key sets.** A green twin, a wrong-reason red twin, a red live cell, two live rows for one surface and lane, or two twins planting the same mutation are refusals: the checker declines to derive a status rather than rendering a wrong one. Break it and the status table starts reporting credit nobody earned.
- **A twin row's filename carries its mutation.** Two twins for one surface in the same second otherwise collide and one silently overwrites the other, which reads downstream as a surface that only ever had one twin.
- **No status literal may appear inside a row, and status is computed at build time.** Break it and status becomes authored.
- **A row emitted from a dirty worktree is invalid.** The runner uses the real repository state, so the operator must commit before rows count; tests use temporary repositories and are unaffected.
- **Unevaluable propagates and never collapses.** A zero evaluated denominator forces unevaluable; a twin over an unevaluable live cell is unevaluable with the same reason; a denied inventory service is unevaluable rather than empty. Break it and a gate that could not run reads as a pass.
- **All JSON is written with sorted keys, LF, and a trailing newline, and hashes are taken over LF-normalised bytes.** Break it and content hashes and the committed-run bindings false-alarm on checkout.
- **Every AWS mutation, quota request, image push, and destroy is an operator-approved action**, and the lane-2 kill switch fires on wall-clock time rather than on a condition. Break it and an unattended failure bills a GPU overnight.
- **The public-repo wording rule holds for every new document**, including this one.

## Open / next

1. **Commit the pending tree.** Four modified files, this brief, five learnings entries, and the index are uncommitted. Command below; the records under `docs/handoff/` and `docs/learnings/` written before the wording ruling keep their wording deliberately.
2. **Create both AWS profiles.** This is the first hard blocker: `traverse-platform` from the `traverse-platform-admin` user's key and `traverse-provision` from the provision-admin group's user. Nothing in Parts 2 or 3 runs without them, and the plan's preflight is the first thing that will say so.
3. **Accept the corpus mirror's terms and export a token.** The mirror is gated, which the plan's operator-input table originally called optional and now states correctly. Without an accepted token the staging step returns 401 partway through Part 2's runbook, on a running instance.
4. **Two permission decisions that shape Part 3.** Whether `traverse-provision` is granted `eks:*` (without it, Part 3 self-reports every lane-2 surface as unevaluable with a stated reason and still builds and validates offline), and whether `traverse-platform` is granted `tag:GetResources` and `elasticloadbalancing:Describe*` (without them the teardown probe falls back to per-service listing and marks the services it cannot read unevaluable).
5. **Then execute the plan in order**, Part 0 through Part 3, with a fresh agent per task and a review between tasks. Part 0 has one known interaction with this brief: its Task 0.4 replaces the whole body of `docs/handoff/HANDOFF.md`, and the replacement text in the plan has been extended to keep this brief's pointer row alongside the earlier one. Check that it did before running that task.
6. **A note for whoever writes the next brief:** the 2026-09-07 handoff and four learnings entries name the private governing text by path, commit, and rule number. They are records and stay as written; superseding entries should carry the corrected form rather than edits.

**Checkpoint (operator runs):**
```bash
cd ~/dev/traverse
git add README.md docs/2026-09-05-design.md docs/2026-09-06-datum-gaps.md docs/discipline.md
git commit -m "docs: re-align design to the governing text's current row and status form"
git add docs/handoff docs/learnings
git commit -m "docs: handoff and learnings for the full build plan"
git push
```
Verified before writing: the working tree state above by read-only `git status`; the remote by `git remote get-url`. The remote's own tip was not re-read, so `git push` covers either case.
