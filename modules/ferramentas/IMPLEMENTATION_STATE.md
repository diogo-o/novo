# Ferramentas — Implementation State

Evidence only. This document never creates a functional rule. External references point to
DMO-MODULAR source that is not present in this repository and is not pinned to a commit.

## INDIRECT EVIDENCE

- Job On and Controlo evidence relies on resolving existing Tools and creating frozen context rows
  with the canonical `tool_id`. That presupposes a working Tool master surface, but it is not
  direct Ferramentas evidence and must not be read as proof of the Ferramentas UI or rules.

## PARTIAL OR CONFLICTING EVIDENCE

- **Navigation classification discrepancy.** The functional model treats Ferramentas as a Beta
  module; the current DMO catalog labels its access units contextual-only. This is an implementation
  discrepancy to resolve, not a reason to change the functional model.

## UNKNOWN

- No module-local verified Ferramentas evidence is recorded in this pack. Do not infer Ferramentas
  readiness from the archived raw reconstruction material.

## Evidence references

- `DMO-MODULAR/src/DMO.Application/Access/ModuleCatalog.cs`
- `DMO-MODULAR/src/DMO.Application/Access/ModuleRegistrations.cs`
