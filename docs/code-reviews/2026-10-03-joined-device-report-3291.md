# Review: joined-device report keys (ut-docs#3291)

- Date: 2026-10-03 · Lane: lane:cloud-24 · Author: Opus 5.5 · Reviewer: Sonnet 5 (independent)
- Core change: universaltill/universal-till#1649 (merged). Its `main` run of
  `lang-pack-drift` names this pack.

Two new core keys are translated:
`issuereport.status.pending_reason.joined_awaiting_credential` and
`issuereport.status.failing_reason.joined_no_access`. The version is bumped
to 1.0.20.

Findings:
1. Medium, fixed: "nuvem" became "cloud", which this file already uses
   throughout (27 uses, no "nuvem").
2. Low, fine as is: "online" and "ligada".
3. Low, fixed: "a receber" became "a obter", to match the other key.

`check-key-drift.sh` (0 drift, 0 orphans) and `validate.sh` pass.
