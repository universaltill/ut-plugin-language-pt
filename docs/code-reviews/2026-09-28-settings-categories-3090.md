# Review: Settings category keys (ut-docs#3090)

**Date:** 2026-09-28 · Built by Opus 5.5 · Reviewed by Fable (independent)

Adds the 19 keys core added in universaltill/universal-till#1513: the Settings landing grid's category tiles (labels and one-line descriptions; Payments reuses `settings.payments.title`), the grid's accessible name, and "Voltar às categorias". Bumps the manifest version so the pack ships. This fixes `lang-pack-drift` on core `main`.

The review confirmed pt-PT wording and the pack's formal register. It asked for the pack's own terms: "funcionários", "tipo de loja", "associar" (not "ligar", which reads as "switch on") and "atualizações de software". All fixed. `validate.sh` and `check-key-drift.sh` pass (0 drift). Core review record: universal-till `docs/code-reviews/2026-09-28-settings-categories-3090.md`.
