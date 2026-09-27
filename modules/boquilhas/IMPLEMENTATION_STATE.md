# Boquilhas — Implementation State

Evidence only. This document never creates a functional rule. External references point to
DMO-MODULAR source that is not present in this repository and is not pinned to a commit.

## IMPLEMENTED EVIDENCE

- Boquilhas domain: the current domain closes the movement vocabulary to `Saida`, `Entrada`, and
  `EntradaSemReparacao`.
- A provisional register can be anchored to an existing BQ Tool and later associated to the matching
  BQ Job On context without rewriting its movement facts.
- Boquilhas settings: the current surface owns repairer registration and independent machine
  assignments for B1 through C3; historical movements keep the repairer used at the time.

## SUPERSEDED

- Older manual wording still describes superseded lifecycle concepts (normal BQ lifecycle, an
  irreparable state, close/reopen buckets). The functional document in this repository is the clean
  rule. Do not revive the older lifecycle. See `governance/LEGACY_CONTAMINATION.md`.

## Evidence references

- `DMO-MODULAR/src/DMO.Domain/Boquilhas/MovementKind.cs`
- `DMO-MODULAR/src/DMO.Domain/Boquilhas/BoquilhaRegister.cs`
- `DMO-MODULAR/src/DMO.Web/Pages/Boquilhas/Definicoes.cshtml`
