# Controlo — Implementation State

Evidence only. This document never creates a functional rule. External references point to
DMO-MODULAR source that is not present in this repository and is not pinned to a commit.

## IMPLEMENTED EVIDENCE

- Comparison backend/domain/persistence: the current Beta model, `comparacao_id`, persistence,
  migration, repository, service, validator, measurement rows, individual `Manter` /
  `Colocar de parte` decisions, reuse of the existing `cm_id`, and unit/integration tests are
  implemented.
- The backend does not use a previous-Peso relation.

## PARTIAL OR CONFLICTING EVIDENCE

- **Resumo:** a real landing/read surface exists. The current Resumo page deliberately shows
  Pegamentos as unavailable, and its comments state that Comparison is reached through Peso rather
  than as an independent route.
- **Peso:** implementation seams exist.
- **Comparison UI/web/navigation:** the current Beta backend is implemented, but the user-facing web
  wiring and navigation are not implemented.

## NOT IMPLEMENTED OR NOT PROVEN

- **Pegamentos:** no current DMO surface proves the Beta workflow. One-axis input must remain valid
  when Contra costura is absent.
- **ComparisonComposer:** DEAD / LEGACY ONLY. Its current/previous Peso relation is superseded and is
  not evidence for the current Comparison model. Do not revive, call, or extend it. See
  `governance/LEGACY_CONTAMINATION.md`.
- End-to-end acceptance across every Controlo document and every production field is not proven.

## Evidence references

- `DMO-MODULAR/src/DMO.Web/Pages/Controlo/Resumo.cshtml`
