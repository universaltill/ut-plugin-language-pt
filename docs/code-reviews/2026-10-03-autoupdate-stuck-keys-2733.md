# Review: auto-update stuck chip keys (ut-docs#2733)

- Date: 2026-10-03 · Lane: lane:cloud-24 · Author: Opus 5.5 · Reviewer: Sonnet 5 (independent)
- Core change: universaltill/universal-till#1638 (merged). Its `main` run of
  `lang-pack-drift` names this pack (de/es were done in their #370 PRs).

Two new core keys are translated: `settings.update.auto_blocked` and
`status.update_auto_blocked`. The version is bumped to 1.0.21.

Findings (no high/medium):
1. Low, fixed: "a pasta de instalação não tem permissão de escrita para a caixa"
   read as a calque; now "a caixa não tem permissão de escrita na pasta de instalação".
2. Low, fine as is: "Se foi instalada" — subject is the till ("caixa"), as in the
   sentence before.
3. Low, fixed: "reinstale o mais recente" → "reinstale o pacote mais recente".

Terminology (caixa, utilizador, pasta, toque, "procurar atualizações") matches
the rest of `pt.json`; gender agreement after "Atualização disponível vX — " checked.

`check-key-drift.sh` (0 drift, 0 orphans) and `validate.sh` pass.
