# The spec amendments and Plan 1 follow DATUM v1, not the governing v2

ts: 2026-09-07T03:55:00Z
commit: none (traverse has no commits; working tree untracked)
session: https://claude.ai/code/session_011hdAYizVaMEkLF8FU5jzFU
status: refuted-assumption
fact: The assumption "the spec and plan adhere to DATUM" (the session's last completed request) was true only of v1. Against v2 at datum `91be859` the spec's 2026-09-06 amendments and `docs/plans/2026-09-06-plan1-core-pipeline-and-ledger.md` diverge in five places: (1) `traverse_sha`/`traverse_worktree` instead of `gate_sha`/`gate_worktree`; (2) `reason` instead of `unevaluable_reason`; (3) no `schema: datum/gate-verdict/1` field and `stage` instead of `surface`; (4) a fully generated STATUS.md instead of the house rule's hand-written record plus generated block between markers; (5) the spec's amendments were appended in place as if the design were a record, which v2 rule 12 names as the wrong form for a governing text. Also not in the plan: vendoring the Node pack, a PIN file, a README DATUM section, and the zero-denominator-forces-UNEVALUABLE rule.
basis: from ~/dev/traverse, `grep -c` over the plan printed `traverse_sha: 8  gate_sha: 0  unevaluable_reason: 0  'datum/gate-verdict': 0`; over the spec printed `gate_sha=0 traverse_sha=1 unevaluable_reason=0`; spec line 140 reads "STATUS.md is generated, so it carries no corrections".
re-verify: grep -c "traverse_sha" "$HOME/dev/traverse/docs/2026-09-05-design.md"; grep -c "gate_sha" "$HOME/dev/traverse/docs/2026-09-05-design.md"
