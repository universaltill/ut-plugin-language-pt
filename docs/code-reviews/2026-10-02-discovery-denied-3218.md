# Review: tills.discovery.denied (ut-docs#3218)

**Change:** one new key in `locales/pt.json` (the iOS Local Network refusal
message added by universal-till#1606) and a patch version bump. Core `main`'s
`lang-pack-drift` named this pack after #1606 merged.

**Reviewer:** independent model (not the author), 2026-10-02.

Checked: meaning matches the English source, including "instead" ("antes");
"Definições" and "Rede local" match Apple's pt-PT iOS labels; formal
imperatives (ative, permita, procure, introduza) like the other `tills.*`
keys; "código de emparelhamento" is the pack's existing term; pt-PT grammar
and AO1990 spelling; curly quotes match the pack's usual style.

**Findings:** none blocking. One optional nit ("Se acabou de lhe ser pedido"
is a little abstract) stays as is: it mirrors the English "If you were just
asked" and the de/es packs.

**Checks:** `scripts/validate.sh` passes; `scripts/check-key-drift.sh` against
core `main` after #1606: 0 drift, 0 orphans.
