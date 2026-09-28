# ut-plugin-language-pt

European Portuguese (pt-PT) language pack for Universal Till (`canonical_type: "language"`,
ADR-0010). Ships `locales/pt.json`; the till merges it as an overlay on
install — base strings always win on key conflict, so packs add locales but
cannot hijack core text. The nav language switcher picks up PT automatically, and a shop whose
country is Portugal (`pt-PT` default locale) gets this pack offered during
setup (ut-docs#2964).

**Coverage: full parity with core's `web/locales/en.json` (ut-docs#2964,
2026-09)** — every core key has a real European Portuguese translation
(AO1990 spelling; "ecrã", "utilizador", "ficheiro", "talão", "IVA"). The
translation is machine-made by the pipeline's build-time model and has not
yet been reviewed by a native speaker; report an odd phrase as an issue.
`i18n-baseline/pt.untranslated.txt` is the tracked baseline of any future
gap: every core key `pt.json` doesn't yet translate, one per line; it is
currently **empty**. `i18n-baseline/pt.same-as-en.txt` is a second list:
keys where `pt.json`'s value is deliberately identical to core's English
string (proper nouns, brand names, words that are genuinely the same in
Portuguese — e.g. `PIN`, `SKU`, `Total`). Neither file ships to customer
tills — `scripts/package.sh` only bundles `manifest.json`, `locales/`, and
`README.md` (see ADR-0010: a language pack ships `locales/<code>.json` and
nothing else; these are developer bookkeeping, not a shipped asset). A key
this pack doesn't yet cover falls back to English — that fallback chain
(`pt-PT → pt → en → en`) is `I18n.T()` in core's `internal/config/i18n.go`,
not something ADR-0010 itself specifies — so a *future* untranslated key
(core will keep adding them) degrades gracefully rather than breaking the
till, but is still a real, tracked gap the moment it appears.

Both files are guarded by `scripts/check-key-drift.sh`, which compares
`locales/pt.json` against core's base locale (fetched from the public
`universal-till` repo, or a local file via `UT_CORE_EN_JSON` for
offline/CI use) and fails loudly if:
- core has gained an untranslated key that isn't already in the baseline
  (new drift) — with the baseline currently empty, this means **any** new
  untranslated key fails CI immediately: exact parity, enforced, not just
  claimed;
- a baseline entry has since been translated, or core dropped the key
  (stale — must be pruned);
- `pt.json` has an orphan key core no longer has;
- a `pt.json` value is empty or whitespace-only (core's `T()` renders it
  unconditionally, so an empty value is blank UI in production — worse
  than the English fallback a missing key would give you);
- a `pt.json` value is byte-identical to core's English value and the key
  is NOT in the same-as-English allowlist (this is the literal
  ut-docs#292 bug: a key present with the verbatim, never-translated
  English string passes a key-set-only check);
- an allowlist entry is no longer identical to core, or its key no longer
  exists (stale — must be pruned);
- a value's placeholder tokens (`%s`/`%d`/…, `{{name}}`, `{0}`) don't match
  core's tokens **in the same order** (ut-docs#297) — a dropped, invented,
  or reordered token is a runtime defect (or, for a positional swap, values
  printed into the wrong slots) even when the key "looks" translated.

Neither file can grow *silently* — new entries only land through a
deliberate, reviewed edit (`scripts/check-key-drift.sh --update-baseline` /
`--update-allowlist`, run and then reviewed like any other diff). That is
a different, weaker, and true claim than "only shrinks": the guard's own
documented escape hatch is adding an entry on purpose, which is not a
violation of it. **With the baseline at full parity, that escape hatch
needs an extra, explicit step (ut-docs#297):**
`--update-baseline` alone refuses to reopen an empty baseline; it requires
`--update-baseline --allow-growth` to deliberately accept new untranslated
debt (e.g. core just shipped a batch of keys this pack hasn't caught up on
yet) instead of silently regressing out of full coverage by muscle memory.

This is what should have caught ut-docs#292: core added 6 `pfand.*` keys,
the German pack was never updated, and a German till silently rendered English
with no CI signal anywhere. CI runs this guard on every push/PR *and* on a
weekly schedule (`.github/workflows/ci.yml`, `key-drift` job), so drift
introduced purely by a core change (no push to this repo at all) still
surfaces — with the caveat that GitHub disables scheduled workflows on
public repos after 60 days of repo inactivity, so the cron is a
best-effort backstop, not a durable guarantee; it also has
`workflow_dispatch` for a manual/pipeline-triggered run. `release.yml`
runs the same guard before a tag can publish, so drift can't ship in a
new pack version either. `scripts/check-key-drift.test.sh` covers the
guard itself.

`scripts/validate.sh` (run by CI and by `package.sh`) additionally asserts
every value in every `locales/*.json` file is a non-empty string — core's
`internal/plugins/syncLocales` unmarshals each file into
`map[string]string` and, on parse error, logs and skips the **entire
file**, so a single non-string value would silently drop all Portuguese
translations on every till while CI stayed green if this didn't catch it
first.

Release: bump manifest version, tag `v<version>` → CI validates, checks
key drift, packages, publishes to the marketplace, auto-approves (dev).
