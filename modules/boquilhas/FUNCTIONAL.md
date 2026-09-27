# Boquilhas — Functional Rules

Functional authority for Boquilhas. Access is in `shared/ACCESS_MODEL.md`; identities in
`shared/IDENTITY_AND_RELATIONS.md`.

## 1. Purpose and ownership

Boquilhas owns the manual external-repair movement register for BQ and its repairer settings. It does
not own the BQ master, production planning, or physical stock.

## 2. Pre-Job On association

Before a Job On exists, a register may be provisionally anchored to an existing canonical BQ
`tool_id`. When the matching BQ context is confirmed, the same register is associated to that
`bq_id`; the provisional Tool anchor is cleared, the version advances once, and movements already
recorded remain unchanged. No fake Job On or fake BQ context is created.

## 3. Closed movement vocabulary

Only these movements are valid:

1. **Saída** — BQ sent to the external repairer.
2. **Entrada** — repaired BQ returned.
3. **Entrada sem reparação** — BQ returned without repair.

There is no extra movement type, no permanent standalone BQ lifecycle, no irreparable Tool state, and
no automatic quantity mutation. The register is quantity-based. Outstanding quantity is derived at
read time as:

```text
Saída - Entrada - Entrada sem reparação
```

It is never stored as a separate balance. A negative result is valid and visible; excess returns are
recorded completely and shown as a non-blocking discrepancy.

## 4. Settings and history

- Repairers are registered and renamed in Boquilhas; history is preserved.
- Machine assignments B1, B2, B3, C1, C2, and C3 are independent.
- Changing a default assignment does not rewrite historical movements.
- Every user granted the module performs the same operational actions; there is no action split
  inside Boquilhas.
- Movement history is append-only in functional effect; corrections add a new fact or an auditable
  cancellation rather than silently rewriting the past.

## 5. Boundaries

- Ferramentas stays the BQ master owner.
- Boquilhas never mutates Job On, Tool master data, or physical stock.
- Excluded domains never appear in Boquilhas navigation or workflows.

## 6. Current implementation state

See `modules/boquilhas/IMPLEMENTATION_STATE.md`. Evidence does not create or override functional
rules.
