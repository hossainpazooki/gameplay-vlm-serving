# A twin row's filename must carry its planted mutation

ts: 2026-09-10T04:46:51Z
commit: c6bafff0487349dc065b85ab7f1e255e4c3384b8
session: https://claude.ai/code/session_01LvzR5v7WVozJWMPtMG7M1F
status: verified
fact: Naming verdict rows `<surface>-lane<lane>-<cell>-<stamp>.json` collides whenever one surface has two twins emitted in the same second, which this design guarantees: the fetch and decode surfaces each get a fixture twin in CI and a runtime twin on the rented instance, and both land in one output directory. The second write silently overwrote the first, so the checker saw one twin where two were planted and the crediting rule was satisfied by a row that no longer existed. Twin filenames now carry the mutation; live rows keep the old shape. The planned build is local-only (`docs/plans/2026-09-09-full-build.md`, git-excluded), so the fix currently lives in plan text, not in repository code.
basis: two twin rows for surface `fetch`, lane 1, both stamped `2026-09-10T04:00:00Z`, written through the planned `write_verdict` in the scratch tree produced `['fetch-lane1-twin-plant_throttled_throughput-20260910T040000Z.json', 'fetch-lane1-twin-tc_throttle-20260910T040000Z.json']`, `distinct: True`, `rows on disk: 2`; without the mutation segment both resolve to `fetch-lane1-twin-20260910T040000Z.json`. Found first as a test failure whose message was `derive_status(...) == "CLAIMABLE" and len(cells[("fetch", 1)]) == 3` reporting `CLAIMABLE and 2 == 3` (that failure pre-dates this entry's anchor and its tree is gone; the demonstration above was re-captured at this entry's timestamp).
re-verify: grep -c "twin-{row" ~/dev/traverse/docs/plans/2026-09-09-full-build.md
