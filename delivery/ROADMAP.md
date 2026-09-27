# Beta Roadmap

Delivery order for the Beta context. Functional rules live in `modules/*/FUNCTIONAL.md` and the
shared documents; implementation evidence lives in `modules/*/IMPLEMENTATION_STATE.md`.

## Rules

- Owner priority is **not** inferred from implementation evidence. Priority sections stay
  placeholders until the Owner supplies them.
- **Verified evidence is not implementation.** An item listed under "Verified evidence" means a
  verification found working source evidence; it does not mean the surface is registered, navigable,
  or accepted end to end.
- This roadmap is not derived from archived raw plans, and archived plans must not be promoted into
  it. See `archive/README.md`.

## NOW

OWNER PRIORITY REQUIRED

## NEXT

OWNER PRIORITY REQUIRED

## LATER

OWNER PRIORITY REQUIRED

## Verified evidence — NOT a delivery status

These items are evidence found in the current DMO-MODULAR source. Read the linked module state
documents before treating any of them as usable.

- Job On service evidence for create, update, date edit, duplication, and frozen CM/MF/BQ Tool
  contexts — `modules/job-on/IMPLEMENTATION_STATE.md`.
- Comparison backend/domain/persistence evidence, including model, migration, repository, service,
  validator, measurement rows, individual decisions, `comparacao_id`, `cm_id` reuse, and tests —
  `modules/controlo/IMPLEMENTATION_STATE.md`.
- Boquilhas closed movement vocabulary and provisional BQ Tool-to-context association —
  `modules/boquilhas/IMPLEMENTATION_STATE.md`.
- Admin Users/Templates source surfaces and application services —
  `modules/admin/IMPLEMENTATION_STATE.md`.
- Peso PDF naming/storage and explicit manual email-send orchestration —
  `shared/DOCUMENTS_AND_EMAIL.md`.

## Known open gaps — unordered, owner priority required

- Pegamentos: no verified implementation evidence.
- Comparison: no user-facing web wiring or navigation.
- Resumo/Folha de Controlo: persisted lifecycle versus read projection is unresolved.
- Documents/PDF/email: end-to-end acceptance not proven.
- Admin Applications/Audit surfaces not proven.
- Module availability and route reachability not verified
  (`ModuleRegistrations.CurrentBuildAvailable`).
- Ferramentas navigation classification discrepancy (module versus contextual-only).
- No completed end-to-end Beta release with all included modules registered, gated, navigable, and
  tested together.

## Excluded from this roadmap

No roadmap item may cover physical stock, internal repair, external repair, plugs (Tampões),
design-laboratory, or a global História destination.
