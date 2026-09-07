# Sibling repos map a local codename to a descriptive GitHub slug

ts: 2026-09-07T03:53:56Z
commit: none (traverse has no commits; working tree untracked)
session: https://claude.ai/code/session_011hdAYizVaMEkLF8FU5jzFU
status: verified
fact: The family convention is a one-word local directory codename and a two-to-three-word noun-phrase GitHub slug that names the artifact, never containing the codename: atlas -> regulatory-rule-engine, vantage -> pit-fundamentals-lakehouse, parallax -> pit-revision-examiner, cldd -> closed-loop-default-detection. meridian and baseline use the codename as the slug. traverse's slug is still undecided; the two candidates on the table are gameplay-vlm-serving and vlm-serving-pipeline.
basis: `for d in atlas vantage parallax meridian baseline cldd; do git -C ../$d remote get-url origin; done` from ~/dev/traverse printed `hossainpazooki/regulatory-rule-engine`, `hossainpazooki/pit-fundamentals-lakehouse`, `hossainpazooki/pit-revision-examiner`, `hossainpazooki/meridian`, `hossainpazooki/baseline`, `hossainpazooki/closed-loop-default-detection`.
re-verify: for d in atlas vantage parallax cldd; do git -C "$HOME/dev/$d" remote get-url origin; done
