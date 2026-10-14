# Review — `recovery.safe_mode_this_device_only` (ut-docs#4290)

**Shipped:** one new core key translated into this pack, "O modo de segurança (uma vista só de leitura das vendas de hoje) só pode ser aberto na própria caixa." for core's "Safe mode (a read-only view of today's sales) can only be opened on the till itself." — the note the recovery screen shows instead of the safe-mode link when it is opened from another computer the LAN guard admits (safe mode itself stays loopback-only). Pairs with the universal-till PR for ut-docs#4290. Version bumped to 1.0.73.

**Review:** checked against `reference/translation.md`: no placeholders, wording reuses this pack's existing "Modo de segurança"/"Vendas de hoje"/"caixa" terms from the neighbouring `recovery.*` keys; `scripts/validate.sh` and `scripts/check-key-drift.sh` run against the core branch.

**Verdict:** safe to merge once the core PR is on `main`.
