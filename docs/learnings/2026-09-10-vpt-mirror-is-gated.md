# The gameplay corpus mirror is gated, and its layout is not what its card says

ts: 2026-09-10T04:45:27Z
commit: c6bafff0487349dc065b85ab7f1e255e4c3384b8
session: https://claude.ai/code/session_01LvzR5v7WVozJWMPtMG7M1F
status: refuted-assumption
fact: The planned corpus mirror `zhwang4ai/OpenAI-Minecraft-Contractor` reports `gated: auto`, so a Hugging Face token is required and its owner must have accepted the dataset's terms; an anonymous or un-accepted fetch gets 401, and the plan's operator-input table had called the token optional. Its file layout is also one level deeper than the dataset card's prose summary: `<batch>/videos/<alias>-<id>-<date>-<time>.mp4`, for example `all_10xx_Jun_29/videos/cheeky-cornflower-setter-02e496ce4abb-20220421-092639.mp4`, not `<recorder-version>/<name>.mp4`. Path flattening is unaffected, but sorting by path and taking a byte budget draws a contiguous slice from one batch directory rather than a sample across the corpus, which the fetch row's scope must state. Current revision `92f3815a2b524dd43cf6f26b6b52936bc3e5efa7`, licence `mit`, 68,012 files of which 36,075 are `.mp4`.
basis: `curl -s https://huggingface.co/api/datasets/zhwang4ai/OpenAI-Minecraft-Contractor` parsed for the fields printed `sha: 92f3815a2b524dd43cf6f26b6b52936bc3e5efa7`, `licence: mit`, `siblings listed: 68012`, `mp4 in listing: 36075 ['all_10xx_Jun_29/videos/cheeky-cornflower-setter-02e496ce4abb-20220421-092639.mp4', ...]`, `gated: auto private: False`. The refuted claim came from the dataset card's own summary, read 2026-09-09.
re-verify: curl -s https://huggingface.co/api/datasets/zhwang4ai/OpenAI-Minecraft-Contractor | python -c "import json,sys; d=json.load(sys.stdin); print(d.get('gated'), (d.get('cardData') or {}).get('license'), d.get('sha'))"
