# 2026-09-29 — Shop name card keys (ut-docs#3115)

Adds the six keys core introduces in universaltill/universal-till#1526:
`settings.store_name.{title,help,error_required,error_too_long,error_invalid_chars}`
and `elevation.summary.store_name`. Manifest "version": "1.0.6".

Filed as a lane-owned follow-up (no card): universal-till#1526 merged and
`lang-pack-drift` went red on `main` for this pack (no PR existed yet, unlike
`ut-plugin-language-de`/`-es` which already had one queued). SKILL.md's
"Work with no card" rule — the lane that merges a core `web/locales/en.json`
change owns the pack follow-ups in the same cycle.

- `error_required` reuses the pack's existing `setup.error.store_name_required`
  string verbatim (same requirement, same wording, matching the `de`/`-es`
  packs' own choice for this key).
- `elevation.summary.store_name` follows the existing
  `elevation.summary.till_name` pattern (`Mudar o nome desta caixa para
  "%s".`) for the analogous rename action, straight double quotes around
  `%s` per this pack's established convention (not the curly “ ” quotes used
  elsewhere in the file for non-rename strings).
- `%s` placeholder kept (token check passes).
- `scripts/validate.sh` passes. `scripts/check-key-drift.sh` reports all
  2952 core keys translated, 0 drift, 0 orphans, 0 token mismatches.
- No `i18n-baseline/pt.*` entries existed for these keys, so nothing to
  prune.

Safe to merge.
