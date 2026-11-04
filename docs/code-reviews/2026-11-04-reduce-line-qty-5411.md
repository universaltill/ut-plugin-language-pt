# Review — pt: `elevation.summary.reduce_line_qty` (ut-docs#5411)

Date: 2026-11-04. Branch `fix/5411-reduce-line-qty` (uncommitted at review time). Author model: Sonnet 5.5. Reviewer: a different model per `reviewer/SKILL.md`. Source of truth: `universal-till` `web/locales/en.json`, `web/i18n-meta/en.meta.json`, `internal/pages/pos_api.go` (~1674).

## What shipped
- `locales/pt.json`: +1 key, `elevation.summary.reduce_line_qty`: "Reduzir a quantidade de %s de %s para %s como %s." (en: "Reduce %s from %s to %s as %s.").
- `manifest.json`: 1.0.134 -> 1.0.135.
- Context: core 94f605e21 added the key without pack follow-ups, so universal-till `main`'s `lang-pack-drift` has been red. This is the pt follow-up (de landed in ut-plugin-language-de#515).

Pre-flight: scoped to a one-key pack change; no code, UI, money, secrets or client names touched; validate, drift, gitlink and version-bump checks run (below); review record committed with the change.

## Verification (run)
- `scripts/validate.sh`: ok (v1.0.135).
- `UT_CORE_EN_JSON=.../web/locales/en.json scripts/check-key-drift.sh`: 3846/3846 core keys translated, 0 drift, 0 token mismatches, 0 ICU mismatches.
- `scripts/check-key-drift.test.sh`: all pass. `scripts/check-no-gitlinks.sh`: ok.
- `scripts/check-version-bump.sh` diffs `origin/main..HEAD`, so it reports "no change" while uncommitted; checked by hand: origin/main is 1.0.134, bump is one patch.
- No `i18n-baseline/` change needed.

## Meaning and wording
- Variety: manifest and README say European Portuguese (pt-PT). The string is variety-neutral and uses "quantidade", "Reduzir", "para" (as in `user_role_change`: "de %s para %s"), consistent with the pack.
- Placeholders: %s x4, same order as English and `pos_api.go`.
- Reason values: Oferta, Anular, Desperdício. "...de 3 para 2 como Desperdício." reads fine; "como Oferta" fine; "como Anular" is an infinitive used as a label, slightly awkward, but identical to the existing `void_comp_waste` ("Remover esta linha como Anular") and comes from the shared `shrinkage.reason.void` value.

## Findings
| # | Severity | Where | Finding | Action |
|---|----------|-------|---------|--------|
| 1 | Nit | `locales/pt.json:763` | Key sits after `void_comp_waste`, not alphabetical (mirrors core). | Accepted. |
| 2 | Low | `shrinkage.reason.void` = "Anular" | "como Anular" reads as a verb. Pre-existing in `void_comp_waste`; fixing means changing the shared label (e.g. "Anulação"), out of scope. | Deferred: optional follow-up card to change the label to "Anulação"; not blocking. |

No blocking findings.

## Verdict
Safe to merge. Land with the es PR so core `main`'s `lang-pack-drift` can go green. Deferred: finding 2 (optional).
