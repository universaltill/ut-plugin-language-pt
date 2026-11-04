# Review — pt: drop the dead `tile_sheet.delete_item*` keys (ut-docs#2710)

Date: 2026-11-04. Branch `chore/2710-dead-locale-keys` (uncommitted at review
time, forked from `origin/main`). Author model: Sonnet 5.5 Dev agent.
Reviewer: Opus 5.5, a different model per `reviewer/SKILL.md`. Source of
truth: `universal-till` `web/locales/en.json` on the companion branch of the
same name.

## What shipped
- `locales/pt.json`: −3 keys, the ones universal-till #2698 left without a
  consumer — `elevation.summary.buttons_delete_item`
  ("Eliminar “%s” do catálogo. …"), `tile_sheet.delete_item`
  ("Eliminar artigo"), `tile_sheet.delete_item_confirm`. 3843 keys remain.
- `manifest.json`: 1.0.135 → 1.0.136 (patch).
- Nothing else: no `i18n-baseline/` change is needed (neither
  `pt.untranslated.txt` nor `pt.same-as-en.txt` listed these keys — checked,
  so no stale entry is created), no README/LICENSE/`package.sh` change.

Pre-flight: shipped-file change → `manifest.json` version bumped (1.0.136,
one patch above tag v1.0.135). `i18n-baseline/`: n/a (keys not listed). Core
side: `web/locales/{en,ar,fa,tr}.json` +
`scripts/ci/i18n-baseline/key-meta-baseline.txt` on the universal-till branch
of the same name, reviewed in `docs/code-reviews/2026-11-04-dead-locale-keys-2710.md`
there. Help: n/a (no Portuguese help in this repo; the keys had no consumer).
Review record: this file.

## Verification (run)
- `scripts/validate.sh`: ok, com.universaltill.language-pt v1.0.136 (pt).
- `scripts/check-no-gitlinks.sh`: ok. `scripts/check-key-drift.test.sh`: pass.
- `UT_CORE_EN_JSON=<edited core en.json> scripts/check-key-drift.sh`:
  3843/3843 core keys translated, 0 drift, 0 orphans, 0 empty values, 0
  untranslated-present, 0 token/ICU mismatches (81 known-same-as-English).
  Also green through core's `check-lang-pack-drift.sh` run against a local
  mirror of this branch.
- `scripts/check-version-bump.sh` against a `git stash create` snapshot
  (it diffs `origin/main..HEAD`, so it reports "no shipped file changed"
  while uncommitted): "version bumped 1.0.135 -> 1.0.136, covering
  locales/pt.json, manifest.json". Re-run after committing.
- `locales/pt.json` parses; no duplicate keys.
- Order check, both directions: this pack against core `main` (keys still
  there) fails with **3 missing keys / new drift**; core's edited `en.json`
  against pack `main` fails with **3 orphan keys**. See the verdict.

## Findings
| # | Severity | Where | Finding | Action |
|---|----------|-------|---------|--------|
| 1 | Non-blocking (merge order) | — | ut-docs#2710 asks for "packs first or together". For a removal that cannot be green: this PR's CI runs `check-key-drift.sh` against core `main`'s `en.json`, which still has the three keys, so the PR fails until core merges. | **Accepted as an order constraint:** merge the universal-till PR first, then this one in the same cycle. Core `main`'s `lang-pack-drift` is red on orphan keys in that window (expected); re-dispatch it once de, es and pt have landed. |
| 2 | Nit (info) | `locales/pt.json` | The removed European-Portuguese wording was a real translation, not a stub — nothing is lost: the delete-from-grid flow now renders `elevation.summary.buttons_remove`, already translated here. | Accepted; verified the replacement key exists and is translated. |
| 3 | Nit (info) | — | No released till build (every `v0.34.*`/`v0.35.*` tag checked in core) consumes these keys, so shipping v1.0.136 to an older till cannot blank a string. | No action. |

No blocking findings.

## Verdict
Safe to merge, **after** the universal-till PR of the same branch name and in
the same cycle (finding 1).
