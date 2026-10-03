# Review — report retention cloud upload keys (ut-docs#574)

- **Date:** 2026-10-03
- **Lane:** lane:cloud-54
- **Version:** 1.0.26

## Change

Adds the 8 new core keys from universal-till#1664, which merged at 9763ba1:

- `settings.retention.{err_invalid_mode, err_needs_subscription, err_use_retention_card, mode_needs_subscription, pending_uploads, upload_refused}`
- `status.report_archive.{hint, refused}`

Also drops the removed `settings.retention.mode_coming_soon` and bumps the version. This clears the failing `lang-pack-drift` check on core `main`; the de and es packs already landed in their own #380 PRs.

## Review

An independent check by a different model: Sonnet reviewed, while Opus wrote the translations. It found all 8 keys correct on:

- meaning;
- pt-PT usage ("a aguardar", "cópia de segurança") and formal register;
- terminology consistent with the pack's existing "caixa", "cloud", "subscrição" and «Conservação de relatórios»;
- `%d` preserved.

No corrections.

## Checks

- `scripts/check-key-drift.sh` against core `main`: 3080/3080, 0 drift, 0 orphans.
- `scripts/validate.sh`: ok.

## Verdict

Safe to merge.
