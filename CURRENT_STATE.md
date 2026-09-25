# CURRENT STATE

Implementation evidence for the Beta context. Evidence was checked in the current
`DMO-MODULAR` source only; it is not functional authority.

## IMPLEMENTED EVIDENCE

- **Job On service:** create, update, read, date edits on the same Job On identity, and duplication
  are present. CM/MF/BQ associations resolve existing Tools and create frozen context rows with
  the canonical `tool_id` relation. Duplicate contexts receive new context identities while the
  canonical Tool identity is reused.
- **Boquilhas domain:** the current domain closes the movement vocabulary to `Saida`, `Entrada`,
  and `EntradaSemReparacao`. A provisional register can be anchored to an existing BQ Tool and
  later associated to the matching BQ Job On context without rewriting its movement facts.
- **Boquilhas settings:** the current surface owns repairer registration and independent machine
  assignments for B1 through C3; historical movements keep the repairer used at the time.
- **Comparison backend/domain/persistence:** the current Beta model, `comparacao_id`, persistence,
  migration, repository, service, validator, measurement rows, individual `Manter` /
  `Colocar de parte` decisions, reuse of the existing `cm_id`, and unit/integration tests are
  implemented. The backend does not use a previous-Peso relation.
- **Admin users/templates:** current pages and application services cover user list/create/edit,
  activation, deletion, password-reset request, invite resend, and access-template management.
- **Peso documents:** deterministic naming, server-host storage, directory creation, no silent
  overwrite, and explicit existing-file reads are implemented. Manual email sending reuses the
  existing PDF and resolves machine group B or C to its configured template and recipient list.
- **Access vocabulary:** DMO has a code-owned module catalog and server-side authorization
  abstractions. This is evidence of the access foundation, not proof that every Beta surface is
  available in the current build.

## PARTIAL OR CONFLICTING EVIDENCE

- **Build availability:** `ModuleRegistrations.CurrentBuildAvailable` is empty. The source contains
  future slices and pages, but the current production registration does not yet prove a usable Beta
  surface end to end.
- **Job On UI:** consultation is explicitly read-only; mutation pages exist in source, but full
  payload-to-persistence-to-reload-to-print acceptance is not proven for every production field.
- **Controlo:** Resumo is a real landing/read surface and Peso has implementation seams. The current
  Resumo page deliberately shows Pegamentos as unavailable, and its comments state that Comparison
  is reached through Peso rather than as an independent route.
- **Comparison UI/web/navigation:** the current Beta backend is implemented, but the user-facing
  web wiring and navigation are not implemented.
- **Boquilhas model:** current code follows the closed three-movement and provisional-association
  rules, while older manual wording still describes superseded lifecycle concepts. The Beta manual
  in this repository is the clean rule; do not revive the older lifecycle.
- **Admin:** Users and Templates are surfaced. Applications and Audit pages are not proven by the
  current page inventory, so their Beta availability remains partial.
- **Ferramentas navigation:** the functional model treats Ferramentas as a Beta module; the current
  DMO catalog labels its access units contextual-only. This is an implementation discrepancy to
  resolve, not a reason to change the functional model.
- **Documents:** PDF settings, lists, templates, and sending have source evidence. End-to-end
  acceptance across every Controlo document and every production field is not proven.

## NOT IMPLEMENTED OR NOT PROVEN

- **Pegamentos:** no current DMO surface proves the Beta workflow; one-axis input must remain valid
  when Contra costura is absent.
- **ComparisonComposer:** DEAD / LEGACY ONLY. Its current/previous Peso relation is superseded and
  is not evidence for the current Comparison model. Do not revive, call, or extend it.
- **Full Admin Applications/Audit:** no current page inventory proves the required catalog editing,
  audit filtering, and annual export surfaces.
- **Beta acceptance:** there is no evidence yet of a completed end-to-end Beta release with all
  included modules registered, gated, navigable, and tested together.

## CURRENT WORK

- Maintain the Beta-only context pack and its canonical identity/ownership boundaries.
- Keep Comparison backend/domain/persistence separate from its missing UI/web wiring.
- Record remaining implementation gaps without assigning an owner priority.
- Keep excluded modules absent from Beta code paths, navigation, docs, and roadmap items.

## Evidence References

- `DMO-MODULAR/src/DMO.Application/JobOn/JobOnService.cs`
- `DMO-MODULAR/src/DMO.Infrastructure/Persistence/JobOnRepository.cs`
- `DMO-MODULAR/src/DMO.Domain/Boquilhas/MovementKind.cs`
- `DMO-MODULAR/src/DMO.Domain/Boquilhas/BoquilhaRegister.cs`
- `DMO-MODULAR/src/DMO.Web/Pages/Boquilhas/Definicoes.cshtml`
- `DMO-MODULAR/src/DMO.Web/Pages/Controlo/Resumo.cshtml`
- `DMO-MODULAR/src/DMO.Application/Access/ModuleCatalog.cs`
- `DMO-MODULAR/src/DMO.Application/Access/ModuleRegistrations.cs`
- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfNaming.cs`
- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfFileStore.cs`
- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfSendService.cs`
