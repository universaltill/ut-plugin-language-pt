# Review: auto-update stuck chip keys (ut-docs#2733)

**Date:** 2026-10-03 · **Lane:** lane:cloud-24 · **Reviewer:** Fable (independent subagent)

Two core keys from universal-till#1638 (merged as `9cfa771`) go into `locales/pt.json`:
`status.update_auto_blocked` (status chip) and `settings.update.auto_blocked`
(Settings → Check now). `manifest.json` is bumped to 1.0.21. This pack was missed when the
de/es follow-ups were opened, so `lang-pack-drift` on universal-till `main` stayed red for it.

`scripts/validate.sh` and `scripts/check-key-drift.sh` (against core `main`): 3045/3045 keys, 0 drift, 0 orphans.

Translation checks: pt-PT vocabulary ("utilizador", AO90 spelling), formal register as in the
rest of the pack, gender agreement ("a" → atualização, "as" → atualizações, "instalada" →
caixa), "caixa" for till as in the rest of the pack, ".deb" untouched, nothing dropped.

Fixed: optional nit — "a pasta de instalação" → "a sua pasta de instalação" (keeps the source's possessive).

Verdict: safe to merge.
