# INDEX — Beta Context Navigator

This index is the single map of the Beta context pack. It answers three questions:

1. Where does a given piece of information live?
2. Is a document current authority, evidence, reference, or legacy?
3. Which questions are still open?

Read `README.md` first for scope and authority order.

## 1. How to find information

| I need to know… | Read |
|---|---|
| Scope, excluded domains, authority order | `README.md` |
| Where a document belongs and whether it is current | this file |
| Access and authorization model | `shared/ACCESS_MODEL.md` |
| Identities and allowed/forbidden relations | `shared/IDENTITY_AND_RELATIONS.md` |
| Records, PDF, storage, and email flow | `shared/DOCUMENTS_AND_EMAIL.md` |
| Landing, navigation, and module composition | `shared/MODULE_FLOW.md` |
| Shell, tables, selection, information preservation | `shared/FRONTEND_RULES.md` |
| Job On function and rules | `modules/job-on/FUNCTIONAL.md` |
| Job On implementation evidence | `modules/job-on/IMPLEMENTATION_STATE.md` |
| Ferramentas function and rules | `modules/ferramentas/FUNCTIONAL.md` |
| Ferramentas implementation evidence | `modules/ferramentas/IMPLEMENTATION_STATE.md` |
| Controlo function and rules (Peso, Comparison, Pegamentos, Resumo, Histórico) | `modules/controlo/FUNCTIONAL.md` |
| Controlo implementation evidence | `modules/controlo/IMPLEMENTATION_STATE.md` |
| Boquilhas function and rules | `modules/boquilhas/FUNCTIONAL.md` |
| Boquilhas implementation evidence | `modules/boquilhas/IMPLEMENTATION_STATE.md` |
| Admin function and rules | `modules/admin/FUNCTIONAL.md` |
| Admin implementation evidence | `modules/admin/IMPLEMENTATION_STATE.md` |
| Delivery order and open gaps | `delivery/ROADMAP.md` |
| Concepts that must never be reintroduced | `governance/LEGACY_CONTAMINATION.md` |
| Why the context was reorganized | `governance/CONTEXT_DECISIONS.md` |
| Historical or raw material (never authority) | `archive/README.md` |

## 2. Repository structure

```text
README.md                               scope, authority, working rules
INDEX.md                                this navigator
modules/job-on/                         Job On
modules/ferramentas/                    Ferramentas
modules/controlo/                       Controlo, including all its internal areas
modules/boquilhas/                      Boquilhas
modules/admin/                          Admin
shared/ACCESS_MODEL.md                  user -> access template -> modules/permissions
shared/IDENTITY_AND_RELATIONS.md        canonical identity graph and relation rules
shared/DOCUMENTS_AND_EMAIL.md           record -> PDF -> storage -> email
shared/MODULE_FLOW.md                   session, landing, navigation, module composition
shared/FRONTEND_RULES.md                shell, table, and interaction constraints
delivery/ROADMAP.md                     ordered delivery plan and open gaps
governance/LEGACY_CONTAMINATION.md      registry of concepts that must not return
governance/CONTEXT_DECISIONS.md         record of the corrections applied here
archive/                                raw and historical material, never authority
```

Organization principle: information is grouped by module. It is not grouped by date, history,
document origin, author, or tool.

## 3. Module index

| Module | Functional authority | Implementation evidence | Uses shared |
|---|---|---|---|
| Job On | `modules/job-on/FUNCTIONAL.md` | `modules/job-on/IMPLEMENTATION_STATE.md` | access, identity, documents, module flow |
| Ferramentas | `modules/ferramentas/FUNCTIONAL.md` | `modules/ferramentas/IMPLEMENTATION_STATE.md` | access, identity, module flow |
| Controlo | `modules/controlo/FUNCTIONAL.md` | `modules/controlo/IMPLEMENTATION_STATE.md` | access, identity, documents, module flow |
| Boquilhas | `modules/boquilhas/FUNCTIONAL.md` | `modules/boquilhas/IMPLEMENTATION_STATE.md` | access, identity, module flow |
| Admin | `modules/admin/FUNCTIONAL.md` | `modules/admin/IMPLEMENTATION_STATE.md` | access, module flow |
| Cross-module | `shared/ACCESS_MODEL.md`, `shared/IDENTITY_AND_RELATIONS.md`, `shared/DOCUMENTS_AND_EMAIL.md`, `shared/MODULE_FLOW.md`, `shared/FRONTEND_RULES.md` | evidence sections inside `shared/ACCESS_MODEL.md` and `shared/DOCUMENTS_AND_EMAIL.md` | — |

Internal Controlo areas (Peso, Comparison, Pegamentos, Resumo/Folha de Controlo, Histórico) are not
modules. They live in `modules/controlo/FUNCTIONAL.md`; they must not be given their own module
document.

## 4. Document classification

| Document | Class | Notes |
|---|---|---|
| `README.md` | ACTIVE | Scope, authority, working rules. |
| `INDEX.md` | ACTIVE | This navigator. |
| `modules/*/FUNCTIONAL.md` | ACTIVE | Functional authority, organized by module. |
| `modules/*/IMPLEMENTATION_STATE.md` | ACTIVE | Implementation evidence only; never functional authority. |
| `shared/ACCESS_MODEL.md` | ACTIVE | Canonical access model. |
| `shared/IDENTITY_AND_RELATIONS.md` | ACTIVE | Canonical identity graph. |
| `shared/DOCUMENTS_AND_EMAIL.md` | ACTIVE | Canonical document and email flow. |
| `shared/MODULE_FLOW.md` | ACTIVE | Canonical session/navigation flow. |
| `shared/FRONTEND_RULES.md` | ACTIVE | Confirmed frontend constraints. |
| `delivery/ROADMAP.md` | ACTIVE | Delivery plan; owner priority still required. |
| `governance/LEGACY_CONTAMINATION.md` | ACTIVE | Negative authority: what must not be reintroduced. |
| `governance/CONTEXT_DECISIONS.md` | ACTIVE | Record of the corrections applied to this pack. |
| `archive/README.md` | REFERENCE ONLY | Explains what is archived and why. |
| `archive/AUDIT_REPORT_CONTEXT_INTEGRITY.md` | REFERENCE ONLY / SUPERSEDED | Historical read-only audit; its findings are resolved or tracked by the current pack. |
| `archive/qwen/DMO_BETA_IMPLEMENTATION_STATE_RECONSTRUCTION_ROADMAP_RAW.md` | REFERENCE ONLY | Raw external-DMO-MODULAR plan; not authority, not this repository's roadmap. |
| `MANUAL.md` (git history only, deleted in `a465195`) | LEGACY CONTAMINATION | Full-scope manual: excluded domains and the old profile model. Do not read as current. |
| `BETA_FUNCTIONAL_MANUAL.md` (previously active) | SUPERSEDED — functional content ACTIVE and redistributed; its section 2 profile model was LEGACY CONTAMINATION | Split into `modules/*/FUNCTIONAL.md` and the shared functional documents; preserved at git `e56ef46`. |
| `CURRENT_STATE.md`, `ROADMAP.md`, `FRONTEND_RULES.md`, `maps/IDENTITY_RELATIONS.md`, `maps/DOCUMENT_FLOW.md` (previously active) | SUPERSEDED | Evidence split per module, roadmap corrected, maps moved to `shared/`; preserved at git `e56ef46`. |
| `maps/MODULE_FLOW.md` (previously active) | SUPERSEDED / contained LEGACY CONTAMINATION | Its "Profile Variants" table was removed; rewritten as `shared/MODULE_FLOW.md`; preserved at git `e56ef46`. |
| `qwen/DMO_BETA_SECOND_PASS_EVIDENCE_DEEPENING_RAW.md`, `qwen/PEGAMENTOS_RECONCILIATION_FOCUSED_EVIDENCE_RAW.md` | LEGACY CONTAMINATION (removed) | Committed tool-error stubs with no evidence; deleted to avoid false implication that a reconciliation happened. |
| `test_probe.txt` (git history only) | SUPERSEDED | Throwaway probe; ignore. |

## 5. Concept classification summary

| Concept | Class | Handled in |
|---|---|---|
| Template-based access (`User -> access template -> modules/permissions`) | ACTIVE | `shared/ACCESS_MODEL.md` |
| Canonical identities `tool_id` / `jobon_id` / `cm_id` / `mf_id` / `bq_id` | ACTIVE | `shared/IDENTITY_AND_RELATIONS.md` |
| Controlo as one module with internal areas; Comparison inside Peso | ACTIVE | `modules/controlo/FUNCTIONAL.md` |
| Closed Boquilhas movement vocabulary (Saída / Entrada / Entrada sem reparação) | ACTIVE | `modules/boquilhas/FUNCTIONAL.md` |
| Record is the source of truth; PDF derived; email only distributes | ACTIVE | `shared/DOCUMENTS_AND_EMAIL.md` |
| Fixed business profiles `Operador/Controlador` and `Responsável` used as access architecture | LEGACY CONTAMINATION | `governance/LEGACY_CONTAMINATION.md` |
| `previous_peso_id`, `production_id`, `ComparisonComposer` | LEGACY CONTAMINATION | `governance/LEGACY_CONTAMINATION.md` |
| Old Boquilhas lifecycle (`Início` / `Irreparável` / close-reopen) | SUPERSEDED | `governance/LEGACY_CONTAMINATION.md` |
| Excluded modules: Armazém, Reparação Interna, Reparação Externa, Tampões, Design Laboratório, global História | LEGACY CONTAMINATION | `governance/LEGACY_CONTAMINATION.md` |
| Raw Qwen reconstruction roadmap (R-01…R-08) as a live plan | REFERENCE ONLY | `archive/README.md` |
| Deleted full-scope `MANUAL.md` | LEGACY CONTAMINATION | `governance/LEGACY_CONTAMINATION.md` |
| Peso reading pairing by table/row position (old manual) | UNKNOWN | `modules/controlo/FUNCTIONAL.md`, section "Open unknowns" |
| Exact permission identifiers in the module catalog | UNKNOWN | `shared/ACCESS_MODEL.md` |
| Current value of `ModuleRegistrations.CurrentBuildAvailable` | UNKNOWN | `shared/ACCESS_MODEL.md` |

## 6. Open unknowns

These are recorded as open. Do not resolve them by inference.

1. Exact permission catalog (identifiers and granularity) of the code-owned DMO-MODULAR catalog.
2. Current value of `ModuleRegistrations.CurrentBuildAvailable`; Beta end-to-end availability is not
   proven.
3. Whether Ferramentas is a top-level module or contextual-only in the current DMO catalog.
4. Pegamentos workflow: no verified implementation evidence.
5. Comparison user-facing web wiring and navigation.
6. Admin Applications and Audit surfaces.
7. End-to-end acceptance of documents/PDF/email across every Controlo document and production field.
8. Resumo/Folha persisted lifecycle versus the current read projection.
9. Peso reading pairing rule (old positional pairing wording) was not restated in the clean manual.
10. No completed end-to-end Beta release is proven.

## 7. What changed in this reorganization

The previous pack was a flat set of top-level documents plus an audit and raw Qwen input. It
contained two contradictions at the top of the authority chain (a profile-based access model next to
a template-based access canon), an archived raw roadmap that read like a live plan, two empty
evidence stubs, and a roadmap that presented implementation evidence as delivered work.

The pack is now organized by module, with a single index, a single access model, an explicit legacy
registry, and archived non-authority material. `governance/CONTEXT_DECISIONS.md` records each
correction with its rationale. The previous version remains in git at commit `e56ef46`.
