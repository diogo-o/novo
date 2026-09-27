# Access Model

Functional authority for access, authorization, and how a user's behavior varies inside a granted
module.

## 1. Canon

```text
Utilizador → Template de acesso → Módulos/permissões
```

- Access is granted through **access templates**. A template holds the modules available to a user
  and, within those modules, the permissions the user receives.
- Effective access is resolved **server-side** from the canonical module catalog and the user's
  assigned template.
- **Admin** is an assignable system module, not an operational business role.
- There is **no fixed business-role dimension** in the access model. The legacy labels
  `Operador / Controlador` and `Responsável` belong to a previous model and must not be
  reintroduced as architecture, permissions, or navigation. See
  `governance/LEGACY_CONTAMINATION.md`.
- Free-text job titles are visual labels only. They never grant access and never create a variant.
- There is no profile selector at login. The server chooses the landing surface.
- An unassigned module is absent from normal navigation and denied on direct access. Hiding a link
  or a button is never an authorization mechanism.
- A missing active identity, a missing grant, an invalid command, or a stale version fails closed.

## 2. Module actions versus business roles

Behavior that older material described through profile names is, in the current model, a set of
**separate module actions**. The implementation evidence names the Controlo split explicitly
(Controlo Create / Controlo Approve application areas), which is the confirmed shape of the model:
the same module can grant different actions to different users.

| Module | Distinct actions inside the module | Shared action |
|---|---|---|
| Job On | configuration (production data, tooling selection, duplication); check confirmation; consultation | — |
| Controlo | create (measure, record, submit); approve (review, approve, reject, reopen) | — |
| Ferramentas | master maintenance (master data and rules); consultation | — |
| Boquilhas | — | one operational movement-register workflow, identical for every user granted the module |
| Admin | users, templates, applications, audit administration | — |

Rules:

- How a user works inside a granted module depends on which actions the assigned template grants,
  not on a job title or a named role.
- Actions are still validated server-side on every mutating command, together with validation and
  concurrency checks.
- A grant that only allows consultation must not enable a mutating action.

## 3. Open unknown: permission granularity

The canon ends in "modules/**permissões**". This repository does **not** reproduce the permission
catalog's identifiers or its exact granularity. The catalog is code-owned in DMO-MODULAR and is not
pinned here.

Therefore:

- the permission leaf is **UNKNOWN** in this pack;
- do not invent permission identifiers;
- do not map the legacy profile names to permissions;
- until an owner-confirmed catalog is added here, treat any permission-level claim as unverified.

## 4. Current implementation evidence

- **IMPLEMENTED EVIDENCE — access foundation.** DMO has a code-owned module catalog and server-side
  authorization abstractions. This is evidence of the access foundation, not proof that every Beta
  surface is available in the current build.
- **PARTIAL / CONFLICTING — build availability.** `ModuleRegistrations.CurrentBuildAvailable` is
  empty in the recorded evidence. The source contains future slices and pages, but the current
  production registration does not yet prove a usable Beta surface end to end.
- **OPEN DISCREPANCY — Ferramentas classification.** The functional model treats Ferramentas as a
  Beta module; the current catalog labels its access units contextual-only. This is an
  implementation discrepancy to resolve; it is not a reason to change the functional model.

Evidence references (external DMO-MODULAR source, not present in this repository and not pinned to a
commit):

- `DMO-MODULAR/src/DMO.Application/Access/ModuleCatalog.cs`
- `DMO-MODULAR/src/DMO.Application/Access/ModuleRegistrations.cs`

## 5. Boundaries

- Internal Controlo areas (Peso, Comparison, Pegamentos, Resumo, Histórico) are not modules.
- Internal areas and workflows never become separately assignable modules.
- No module may be reachable through a route that bypasses authorization.
- Dormant legacy code never defines an authorization contract.
