# DATUM v2 in ~/dev/datum governs traverse; ~/dev/briefs/DATUM.md is the superseded v1

ts: 2026-09-07T03:54:42Z
commit: none (traverse has no commits; working tree untracked)
session: https://claude.ai/code/session_011hdAYizVaMEkLF8FU5jzFU
status: verified
fact: A separate repo `~/dev/datum` at commit `91be859` holds DATUM v2, whose header reads "governing for three data-platform repositories: baseline, traverse, meridian, by operator ruling of this date." It supersedes the v1 brief at `~/dev/briefs/DATUM.md` (local only). Field-level rulings that bind traverse: outcome reason in `unevaluable_reason` (required iff UNEVALUABLE, forbidden otherwise); emitter identity in `gate_sha` / `gate_worktree`; a `schema` field equal to `datum/gate-verdict/1`; `surface` not `stage`; `rows` and `metrics` optional; STATUS.md is a hand-written dated record plus a generated block between `<!-- datum:status:begin -->` markers checked by `--check`; governed repos vendor a Node conformance pack under `vendor/datum/` with a content-hash PIN. The pack does not exist yet (`conformance/` and `schema/` absent; datum STATUS.md says "Nothing executable exists").
basis: `git -C ~/dev/datum log --oneline -1` printed `91be859 docs: DATUM v2, amendments folded, research clauses added`; `grep -n "governing\|unevaluable_reason\|gate_sha\|between markers" DATUM.md` hit lines 3, 63, 144, 160, 168; `ls conformance schema` printed `No such file or directory` for both.
re-verify: git -C "$HOME/dev/datum" log --oneline -1 && grep -c "unevaluable_reason" "$HOME/dev/datum/DATUM.md" && ls "$HOME/dev/datum/conformance" 2>&1 | head -1
