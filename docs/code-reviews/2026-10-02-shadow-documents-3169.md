# Review: shadow-mode customer-document keys (ut-docs#3169)

**Change:** six new keys from universal-till#1573 (ADR-0124: the sell-screen
shadow banner, the 451 refusal, the Sale recorded card, the read-only Country
settings column), plus the two `permissions.action*.personal_display` keys
from universal-till d6fa6648b that no pack PR carried (they were failing
`key-drift` on every pack PR and `lang-pack-drift` on core `main`).
Merges `main` and bumps the patch version.

**Reviewer:** independent model (Sonnet, not the author), 2026-10-02.

Checked against the English source: meaning and omissions; register
(pt-PT forms (ecrã, talões, Acompanham-no); „ecrã de venda“ and „Tema“ match the pack's existing terms); no placeholder tokens needed; no certification claim.

**Findings:** one, fixed: `permissions.action_desc.personal_display` used „esquema“ for "layout"; changed to „disposição“, the pack's existing term (layout plugin strings).

**Checks:** every core `en.json` key at universal-till `e5d75d7a` is present
in `locales/pt.json`; the JSON parses.
