# BASELINE twin rows carry planted {mutation, mutated_rows, expected_violations}; MERIDIAN credits by exact equality

ts: 2026-09-07T03:53:43Z
commit: none (traverse has no commits; working tree untracked)
session: https://claude.ai/code/session_011hdAYizVaMEkLF8FU5jzFU
status: verified
fact: A committed BASELINE twin row has `cell: twin`, `result: RED`, and a `planted` block with exactly the keys `expected_violations`, `mutated_rows`, `mutation`. MERIDIAN's claimability checker re-derives red-as-planted by `r["checks"] == r["planted"]["expected_violations"]`. This is the twin-crediting mechanic traverse's `gates/ledger.py` must reproduce; the plan's `twin_red_as_planted` compares over the union of both key sets, which is the form DATUM v2 rule 3 states.
basis: `python -c "import json,glob; r=json.load(open(glob.glob('../baseline/ledger/verdicts/*twin*.json')[0])); print(r['cell'], r['result'], sorted(r['planted']))"` printed `twin RED ['expected_violations', 'mutated_rows', 'mutation']`; `grep -n 'r\["checks"\] == r\["planted"\]\["expected_violations"\]' ../meridian/gates/claimability.py` printed line 42.
re-verify: python -c "import json,glob,os; r=json.load(open(glob.glob(os.path.expanduser('~/dev/baseline/ledger/verdicts/*twin*.json'))[0])); print(r['cell'], r['result'], sorted(r['planted']))"
