# Review: initial European Portuguese pack (ut-docs#2964)

**Date:** 2026-09-28 · **Branch:** `feat/2964-pt-pack` · **Built by:** Opus 5.5 (orchestrator; 8 translation subagents) · **Reviewed by:** Fable (independent subagent)

## What shipped
- A new pack repo scaffolded from `ut-plugin-language-de`: the same scripts, CI, release workflows and guards, renamed de→pt. Manifest `com.universaltill.language-pt` v1.0.0, `locales: ["pt"]` (core resolves `pt-PT` to `pt`). The CI cron slot is Wednesday 03:29 UTC, staggered from de (Monday) and es (Tuesday).
- `locales/pt.json`: all 2888 core keys in European Portuguese (AO1990), machine-translated by the pipeline's build-time model against a fixed glossary (caixa = till, posto de caixa = register, talão, fatura, IVA, artigo, ecrã, utilizador, ficheiro, definições). It has not been reviewed by a native speaker yet, and the README says so.
- `i18n-baseline/pt.untranslated.txt` is empty (full parity). `pt.same-as-en.txt` has 65 keys (PIN, SKU, Total, brand and symbology names, "Portugal"…), each reviewed.
- `check-version-bump.sh`: a manifest that is **absent** at the base is a new pack's first release, not a missing bump (review finding 2). One that exists but doesn't parse still fails closed, and an already-tagged first version is still refused. Three new test cases (12–14). This is the shared template, so de/es can adopt it if they're ever recreated.

## Findings (Fable)
| # | Severity | Finding | Outcome |
|---|---|---|---|
| 2 | major | First PR fails `version-bump`: `main` (licence only) has no `manifest.json` | Fixed as above; the test cases failed before the fix, and a real `BASE_SHA=main` run now passes |
| 5 | major | "caixa" was used for both till and register, so the register strings contradicted each other (and `registers.strand_warning` named a menu that doesn't exist) | Fixed: register = "posto de caixa" in `registers.*`, `page.title.registers`, `settings.tills.register_*`, `elevation.summary.till_register`, `locations.error.in_use`, `permissions.action.stock_location_management`, `shifts.register`, `sync.quarantine_reason.unknown_sale_reference`; `page.title.fiscal_register` = "Registo fiscal" |
| 6 | minor | Quoted labels didn't match the real buttons | Fixed: "Consultar saldo", "Procurar uma caixa principal nesta rede", "Mosaicos do menu ocultos" |
| 7 | minor | Same concept, two words | Fixed: "ao balcão" throughout, "para levar" (no "take-away"), "Localização", "morada", "data de início/fim", "stock não importado" |
| 8 | minor | Meaning errors | Fixed: payout = "saída de numerário", customer-erase wording, "Cód. aut.". **Accepted:** `tender.pay_empty` stays "Adicione artigos", because the full phrase clipped on the 1024×600 pay button in the driven run. **Accepted:** `setup.demo_data.hint` keeps "10%-off", because core's English has `%-o`, which the token guard reads as a verb. Rewording it would be a token mismatch |
| 9 | nit | ci.yml comment said "a Portuguese till" for the German #292 incident; several wording nits | Fixed ("German" restored; "retifica a fatura", "removidos", "em cêntimos", "Repetir", "Preencher auto.") |
| — | legal note | `invoice.doc_title` prints "FATURA"; in Portugal only AT-certified software may issue one | Not a translation bug. ADR-0124 (accepted) suppresses every customer document for a PT shop until a certified route exists (#2956, #3173/#3174) |

The core findings (1, 3, 4) are in `universal-till/docs/code-reviews/2026-09-28-pt-language-pack-2964.md`.

## Verified
- `validate.sh`, `check-key-drift.sh` against core `en.json` (2888/2888, 0 drift, 0 token mismatches), `check-key-drift.test.sh`, `check-version-bump.test.sh` and `package.sh` all pass (WSL, from a git clone of the branch).
- The reviewer read every key under `tender.*`, `basket.*`, `receipt.*`, `invoice.*`, `pos.*`, `setup.*` and `registers.*`, plus 260 spread across the file. It found no Brazilian Portuguese (tela, usuário, arquivo meaning file, celular, salvar, cadastro, equipe, você, progressive gerunds). Every "arquivo" means archive.
- Driven run in core (see the core record): nine pages in pt-PT with no English from this pack.

**Verdict:** safe to merge. The first release then needs the product owner to set `MARKETPLACE_BASE_URL` / `MARKETPLACE_UPLOAD_TOKEN` on this repo (variable `AUTO_APPROVE=true` is set at creation).
