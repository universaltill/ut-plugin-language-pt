# Review: pos.error.sale_status_transition + catalog.option_sets.* catch-up (ut-docs#3368)

- **What:** adds `pos.error.sale_status_transition` (core universal-till#1623, ut-docs#3368)
  and the 20 `catalog.option_sets.*` keys core `main` gained with ut-docs#3319 that this
  pack never received; `key-drift` was red on `main` without them. Version 1.0.18.
- **Reviewer:** Fable (independent of the author). All 21 strings checked against
  core's English: meaning, pt-PT usage consistent with the file (artigo, Eliminar,
  Remover, separador, "Esta ação não pode ser anulada", Mudar o nome, Ativar/Desativar),
  `%s`/`%d` preserved in grammatical positions, JSON valid, key order sensible.
- **Findings:** one optional nit — `in_use_refused` said "desses artigos" where
  `used_by_hint` says "destes" for the same English "these items"; harmonised to
  "destes".
- **Verified:** `scripts/validate.sh`; `scripts/check-key-drift.sh` against core
  `main`: 3040/3040 translated, 0 drift, 0 orphans, 0 token mismatches.
- **Verdict:** ok to merge.
