# DMO BETA — IMPLEMENTATION STATE RECONSTRUCTION & ROADMAP

**Authority order applied:**
1. Beta Functional Manual (functional truth)
2. Repository corpus: migrations, code, routes, tests (implementation reality)
3. Plans / reports / dev-responses (historical evidence only)

---

## 1. CURRENT IMPLEMENTATION MAP

### Definitive schema evidence

The single strongest proof of implementation state is the migration chain. The Comparação migration test (`Migration009ControloComparacaoDomainTests.cs`) asserts the exact ordered migration list, proving all ten exist and apply in sequence:

| # | Migration | Proves |
|---|---|---|
| 001 | `20260922001736_AccountAndTemplateFoundation` | `admin_accounts`, `users`, `templates` |
| 002 | `20260922001757_TemplateModuleComposition` | `template_modules` |
| 003 | `20260922232349_ToolJobOnDomainCore` | `tools`, `tool_machines`, `jobons`, `cm_contexts`, `mf_contexts`, `bq_contexts` |
| 004 | `20260923045054_ControloCreateDomain` | `pesos`, `peso_measurement_rows`, `repairers`, `machine_repairer_assignments`, `pdf_directory_settings`, `email_lists`, `email_list_recipients`, `email_templates` |
| 005 | `20260923122429_GlassDensitySettings` | `glass_density_settings` |
| 006 | `20260923171223_ControloApproveDomain` | `peso_review_decisions` |
| 007 | `20260924051151_BoquilhasDomain` | `boquilhas`, `boquilha_movements`, `boquilha_movement_audit` |
| 008 | `20260924130151_BoquilhasPreJobonAssociation` | provisional `tool_id` anchor + association |
| 009 | `20260924182527_EmailTemplateGroupRouting` | `email_templates.machine_group`, `email_list_id` |
| 010 | `20260925071139_ControloComparacaoDomain` | `comparacoes`, `comparacao_cm_subjects`, `comparacao_measurement_rows` |

**Critical conclusion:** `reports/BETA_MASTER_RECONCILIATION.md` (written at `7910f56…`) stating *"DMO-MODULAR is still in the application-foundation phase… contains no operational Beta module whatsoever"* is **definitively STALE**. It predates migrations 003–010. The repository has advanced through P2-T04, P2-T05 (+correction), P2-T06, P2-T07 (+correction), P2-T08, and Comparação. That reconciliation must not be used as current-state evidence.

### Area-by-area classification

| Area | Classification | Concrete proof |
|---|---|---|
| **Access / profiles / templates** | **IMPLEMENTED** | Migrations 001–002; `ModuleCatalog.cs` (13 canonical modules); `ModuleRegistry.cs`; `AccessResolver.cs`; `ModuleAuthorizationHandler.cs`; Admin pages `Pages/Administration/Templates/*`; `DirectRouteEnforcementTests` |
| **Job On** | **IMPLEMENTED** | Migration 003; `JobOnRepository.cs` (incl. `ApplyKeepAsync`/`AddContextAsync` re-inserting ORIGINAL context ids — the duplication snapshot invariant); Job On Razor pages |
| **Ferramentas (Tool master)** | **IMPLEMENTED** | Migration 003 (`tools`, `tool_machines`); `ToolRepository.cs`; `ToolEntityConfiguration.cs`; `FerramentasLightEndpoints.cs`; `ToolValidator` + `ToolValidatorTests` |
| **Controlo / Peso** | **IMPLEMENTED** | Migrations 004–006 (`pesos`, `peso_measurement_rows`, `peso_review_decisions`); Controlo Create endpoints/pages; `PesoRowCalculationRules`; water-density 31-value table; glass-density settings |
| **Controlo / Comparação** | **BACKEND IMPLEMENTED · UI NOT IMPLEMENTED** | Migration 010 + full domain (`Comparacao`, `ComparacaoCmSubject`, `ComparacaoMeasurementRow`, `ComparacaoId`, `ComparacaoCmDecisionKind`) + `ControloComparacaoService` + `ComparacaoRepository` + validator + unit/integration/migration tests. **No Razor page, no endpoint mapping, no UI component for Comparação anywhere in corpus.** |
| **Controlo / Pegamentos** | **NOT IMPLEMENTED** | No Pegamentos migration exists (absent from the 10-migration chain). Corpus confirms `pegamentos_id` is a *"plan-level identity that does not exist in code"* (master plan §8; P2-T05 Q-SCOPE; BND7/AC-Y5). No schema, service, page, or route. |
| **Controlo / Resumo** | **PARTIAL** | Corpus F-04: *"the app implements the Resumo as a read projection only. There is no `resumo` table, column or route."* A read-only Resumo entry surface exists (Razor page referencing `jobon_id` traversal). Persisted `resumo_id` record is an unimplemented handoff remainder. |
| **Controlo / Histórico (local)** | **PARTIAL** | Local per-module history convention exists (P2-T05/T06/T07 histories). Not the deferred HISTÓRICO GLOBAL (`historia` identity preserved, no route — deferred by design). |
| **Controlo / Definições** | **IMPLEMENTED** | Migration 004–005 + 009; `ControloDefinicoesEndpoints.cs` (routes 15–17 PDF/email + 18–19 glass density); repairer register present here in schema (see trap below) |
| **Boquilhas** | **IMPLEMENTED** | Migrations 007–008 (3 movement types, no lifecycle, provisional `tool_id` + association); Boquilhas endpoints/pages; repairer resolution |
| **Admin** | **IMPLEMENTED** | Migrations 001–002; `Users`/`Templates` pages; `UserAdministrationService`, `TemplateAdministrationService`; audit foundation |
| **Documents / PDF / Email** | **PARTIAL** | Schema for PDF directory + email lists/templates/group-routing exists (migrations 004, 009). **Actual PDF generation, file storage, and email SEND logic: NOT proven in corpus.** P2-T08 plan lists `src/DMO.Application/Documents/`, `Infrastructure/Files`, `Infrastructure/Pdf` as *expected* (not-yet-existing) paths. |
| **Navigation / routes / availability** | **FOUNDATION IMPLEMENTED · AVAILABILITY UNCERTAIN** | `DestinationRouteRegistrations.cs`, navigation shell, root routing, landing all exist (P1-T07). **Exact current content of `ModuleRegistrations.CurrentBuildAvailable` cannot be proven from corpus excerpts** (historical docs show `[]`; per-workstream registration rule implies it was populated as surfaces landed). |
| **Shared frontend infra** | **IMPLEMENTED** | P2-T01 (`CommonState`, `RecordStatus`, `AvailabilityState`, partials, `dmo-components.css`); P2-T02 (`DenseDataTable`, `AuditTrail`) |

---

## 2. FUNCTIONAL DELTA (Manual vs. repository)

| Functional Manual requirement | Repository state | Delta |
|---|---|---|
| Comparação: optional workflow inside Peso, `comparacao_id`, reuse `cm_id`, Manter/Colocar de parte, zero-effect on Peso | Backend/domain/persistence **complete** (migration 010 + service + tests) | **UI missing** — no page/route/component to trigger or interact |
| Pegamentos: dimensional control, Costura/Contra-costura, Ovalização, Média, tolerance corridor | **No schema, no service, no UI** | **Entire area missing** |
| Resumo / Folha de Controlo: consolidated five-family sheet, OK/NOK, states | Read projection exists; persisted record + Folha edit/submit/approve states **not proven** | **Folha lifecycle + persistence unproven** |
| Peso: water-density table, glass-density settings, capacity/glass-weight, submit/approve/reject/reopen | **Complete** | None |
| Boquilhas: 3 movements, outstanding replay, provisional `tool_id`, association, Definições repairers | **Complete** | None |
| Job On: NEW context snapshots on create/duplicate, never reuse context ids | **Complete** (`ApplyKeepAsync` re-inserts ORIGINAL ids; duplication reuses `tool_id` only) | None |
| Ferramentas: canonical `tool_id`, CM/MF/BQ master, processo, machines | **Complete** | None |
| Documents: PDF generation in Controlo_Create, `<base>/<ref>/<prod>/` naming, production-date field | Schema only; **generation/storage logic unproven** | **Generation + storage missing/unproven** |
| Email: machine-group B/C routing, templates, recipients, manual send | Group-routing schema exists (009); **send logic unproven** | **Send logic missing/unproven** |
| Admin: users, templates, module assignment, audit | **Complete** | None |

### Legacy traps confirmed present in corpus (must be ignored by agents)

| Trap | Status | Where it appears |
|---|---|---|
| `previous_peso_id` | **DEAD — never exists** | Rejected in Comparação model; `ComparacaoIdentityContractTests.NoPreviousPesoOrProductionIdentityCanBeRepresented` proves absence |
| `production_id` | **DEAD — never created** | Master plan §8 + cleanup response: *"No production_id. No second context layer."* |
| `ComparisonComposer` / `ComparisonCompositionTests` | **DEAD LEGACY** | P2-T06 dormant seam for the rejected `previous_peso_id` concept. Newer Comparação code does not reference it. Do not wire it. |
| Old Boquilhas lifecycle (`Início`/`Irreparável`/close-reopen/4-bucket) | **SUPERSEDED** | Replaced by 3-movement + replay model (migrations 007–008) |
| Controlo repairer ownership | **SUPERSEDED location** | Repairer *register* physically in Controlo_Create schema (migration 004) but **functional ownership = Boquilhas > Definições**. Controlo repairer service surface is dead code (zero callers). |
| `BETA_MASTER_RECONCILIATION.md` "foundation phase" claim | **STALE** | Predates migrations 003–010 |

---

## 3. OLD ROADMAP RECONCILIATION

The previous roadmap was built when the repository was at/near foundation phase. Against current reality:

| Old roadmap step | Classification | Reason |
|---|---|---|
| Admin / Login / Users / Templates | **ALREADY IMPLEMENTED** | Migrations 001–002, Admin pages, services all present |
| Tool creation / Ferramentas Light | **ALREADY IMPLEMENTED** | Migration 003, `ToolRepository`, `FerramentasLightEndpoints` |
| Job On creation / reuse + context snapshots | **ALREADY IMPLEMENTED** | Migration 003, `JobOnRepository` |
| `bq_id` / `cm_id` / `mf_id` creation & reuse | **ALREADY IMPLEMENTED** | Migration 003 context tables + Job On repository |
| Boquilhas persistent records + movements | **ALREADY IMPLEMENTED** | Migrations 007–008 |
| Peso Criar / Peso Aprovar | **ALREADY IMPLEMENTED** | Migrations 004–006 |
| Glass-density / water-density corrections | **ALREADY IMPLEMENTED** | Migration 005 + calculation rules |
| Controlo Definições (PDF dir / email / glass density) | **ALREADY IMPLEMENTED** | Migration 004–005, 009 + endpoints |
| Pegamentos | **STILL REQUIRED** | No implementation exists |
| Resumo | **PARTIALLY IMPLEMENTED** | Read projection only; Folha lifecycle unproven |
| Comparação | **PARTIALLY IMPLEMENTED** | Backend done; UI missing |
| PDFs / documents | **PARTIALLY IMPLEMENTED** | Schema only; generation/send unproven |
| Module availability registration / route wiring | **VERIFY** | Per-workstream rule implies done; exact state unproven |

**Conclusion:** The old roadmap is ~80% obsolete. The backend core is built. The genuine remaining work is concentrated in **Pegamentos, Comparação UI, Resumo/Folha lifecycle, Documents/PDF/email send, and availability verification**.

---

## 4. NEW IMPLEMENTATION ROADMAP

> Ordering reflects real technical dependencies only. Steps marked **[independent]** have no blocking dependency and may run in parallel.

---

### STEP R-01 — Verify & finalise module availability + route registration

**WHY THIS STEP EXISTS**
Every operational route is only reachable if its Module is registered in `ModuleRegistrations.CurrentBuildAvailable` and its destination route is registered. The corpus proves the foundation registry exists but does not prove the exact current registration set. Before any UI work is trusted end-to-end, availability must be confirmed.

**CURRENT STATE**
- `ModuleCatalog.cs` — 13 canonical module identities (COMPLETE).
- `ModuleRegistry.cs`, `AccessResolver.cs`, `ModuleAuthorizationHandler.cs` — enforcement present.
- `ModuleRegistrations.CurrentBuildAvailable` — historical docs show `[]`; per-workstream rule (*"register availability only when the surface exists"*) implies incremental registration happened. **Exact current value unproven.**
- `DestinationRouteRegistrations.cs` — registration seam present.

**FUNCTIONAL TARGET**
Functional Manual §2: a module not assigned must not appear, must not be usable, must not be directly reachable. Each Beta module (Job On, Controlo Create/Approve, Ferramentas contextual, Boquilhas, Admin) must be gated by exactly one canonical Module policy.

**EXACT DELTA**
Confirm (and correct if needed) that every implemented surface has: (a) a `ModuleDefinition` in `CurrentBuildAvailable`; (b) a registered destination route; (c) fail-closed direct-route enforcement.

**DEPENDENCIES**
None (verification) — but must complete before any UI acceptance test is trusted.

**FILES / AREAS TO INSPECT**
`src/DMO.Application/Access/ModuleRegistrations.cs`, `ModuleCatalog.cs`, `src/DMO.Web/Navigation/DestinationRouteRegistrations.cs`, `src/DMO.Web/Endpoints/*`, integration tests `tests/DMO.IntegrationTests/Navigation/`.

**CONTEXT PACK**
- Module ≠ internal area ≠ workflow.
- Access = assigned modules; profile = experience within module.
- Fail-closed: unknown/unavailable ⇒ whole-resolution denial.
- Ferramentas is **contextual-only** (no top-level destination).
- HISTÓRICO GLOBAL (`historia`) is deferred by design — do NOT register.

**LEGACY / TRAPS TO IGNORE**
- Any plan stating `CurrentBuildAvailable = []` as a *target* (that was the foundation state).
- Do not register Armazém / Reparação / Tampões / História.

**DO NOT TOUCH**
- The 13-module canonical vocabulary and `ModuleCatalog` identities.
- The authorization handler / fail-closed resolver.

**ACCEPTANCE CRITERIA**
- Each implemented operational surface returns 200 for a granted user and 403/redirect for ungranted.
- Unregistered/deferred modules return fail-closed.
- `DirectRouteEnforcementTests` remain green.

**FOCUSED TESTS**
- Existing: `DirectRouteEnforcementTests`, navigation integration tests.
- New: per-module availability + route reachability assertion.

---

### STEP R-02 — Pegamentos: schema + domain + service  **[backend-only]**

**WHY THIS STEP EXISTS**
Functional Manual §6.3 defines Pegamentos as a required Controlo internal area. The repository has **no Pegamentos migration, entity, service, or route**. This is the largest genuine backend gap.

**CURRENT STATE**
- No `pegamentos` table in any of the 10 migrations.
- `pegamentos_id` listed as a plan-level identity that does not exist in code.
- Peso infrastructure (`PesoRowCalculationRules`, Controlo Create area) exists and is the correct integration neighbour.

**FUNCTIONAL TARGET** (Manual §6.3)
- Axes: Costura = 0°, Contra costura = 90°.
- `Ovalização = Costura − Contra costura` (sign preserved).
- `Média = (Costura + Contra costura) / 2`.
- Single-axis allowed: Contra costura absent ⇒ Ovalização undefined, Média = Costura, tolerance applies to Média.
- Tolerance corridor: `Nominal − 0.20` to `Nominal + 0.20`; equality at limit = alert.
- Applies independently to CM, BQ, MF.
- Alerts never block production.

**EXACT DELTA**
Create the Pegamentos domain + persistence + service: identity, measurement rows, nominal, tolerance evaluation, single-axis handling.

**DEPENDENCIES**
R-01 (module/availability context). Consumes Job On context (`cm_id`/`mf_id`/`bq_id`) already present.

**FILES / AREAS TO INSPECT**
`src/DMO.Domain/Controlo/`, `src/DMO.Application/ControloCreate/`, `src/DMO.Infrastructure/Persistence/` (+ one new migration), `src/DMO.Web/Endpoints/`.

**CONTEXT PACK**
- Identity: `pegamentos_id` anchored to Job On context, same pattern as `peso_id` (`cm_id` XOR `tool_id`).
- Reuse the Controlo Create application-area pattern (`{Area}Models.cs` + `{Area}Validator.cs` + `I{Area}Service.cs` + `{Area}Service.cs`).
- One new additive migration; do not touch migrations 001–010.
- Tolerance is an **alert**, never a block.
- CM/MF/BQ measured independently; never share nominals.

**LEGACY / TRAPS TO IGNORE**
- Do not invent a generic "component ID" — use existing `cm_id`/`mf_id`/`bq_id`.
- Do not add `previous_*` relations.
- Ignore any old plan implying Pegamentos is part of Peso.

**DO NOT TOUCH**
- Peso tables/calculations.
- Existing Controlo Create endpoints.

**ACCEPTANCE CRITERIA**
- Pegamentos record can be created against a Job On context.
- Ovalização/Média computed correctly including single-axis case.
- Tolerance corridor flags at-limit and beyond-limit as alerts without blocking.

**FOCUSED TESTS**
- New: calculation tests (both axes, single axis), tolerance boundary tests (at-limit equality), independent-application tests (CM/BQ/MF).
- Existing: Controlo Create regression must stay green.

---

### STEP R-03 — Pegamentos: UI surface  **[frontend-only, depends R-02]**

**WHY THIS STEP EXISTS**
Backend alone is not usable. The Functional Manual requires an operable Pegamentos area within Controlo.

**CURRENT STATE**
No Pegamentos page or partial exists.

**FUNCTIONAL TARGET** (Manual §6.3)
Measurement entry per axis, computed Ovalização/Média display, tolerance alert presentation, independent CM/BQ/MF handling.

**EXACT DELTA**
Razor page + partials + JS wiring inside the Controlo area, reusing shared frontend components (`DenseDataTable`, `MeasurementRows`, `CommonState`).

**DEPENDENCIES**
R-02 (backend must exist first).

**FILES / AREAS TO INSPECT**
`src/DMO.Web/Pages/Controlo/`, `src/DMO.Web/Frontend/Controlo/`, `wwwroot/js/dmo-controlo.js`, `wwwroot/css/dmo-components.css`.

**CONTEXT PACK**
- Reuse `DenseDataTable` + `AuditTrail` + `MeasurementRows` (P2-T01/T02) — do not rebuild.
- Empty ≠ lookup-failed ≠ unavailable ≠ permission-denied (shared state vocabulary).
- Fixed-desktop layout obligations apply.

**LEGACY / TRAPS TO IGNORE**
- Do not fork the Peso renderer; reuse the shared read-model pattern.

**DO NOT TOUCH**
- Peso UI.

**ACCEPTANCE CRITERIA**
- Operator can enter Costura/Contra costura, see computed Ovalização/Média, see tolerance alerts.

**FOCUSED TESTS**
- New: rendered-state tests; single-axis rendering test.
- Existing: shared-component rendering tests stay green.

---

### STEP R-04 — Comparação: UI surface  **[frontend-only]**  **[independent of R-02/R-03]**

**WHY THIS STEP EXISTS**
Functional Manual §6.2 + prior reconciliation confirm Comparação backend/domain/persistence is complete but **no user-facing UI exists**. This is a known, explicit gap.

**CURRENT STATE**
- Backend complete: migration 010, `ControloComparacaoService`, `ComparacaoRepository`, validator, tests.
- No Razor page, no endpoint mapping, no component for Comparação.

**FUNCTIONAL TARGET** (Manual §6.2)
- Start a comparison event against an approved/decided Peso.
- Add one or several CM subjects (reusing existing `cm_id`).
- Record measurements (same Peso calculation, frozen facts).
- Assign individual `Manter` / `Colocar de parte` (justification required for the latter).
- Confirm only when every measured CM has a decision.
- Zero effect on original Peso must be visually and functionally preserved.

**EXACT DELTA**
Expose the existing service through endpoints + a Comparação UI region within the Peso/Controlo surface.

**DEPENDENCIES**
R-01 (availability/gating). Consumes already-implemented Peso read model.

**FILES / AREAS TO INSPECT**
`src/DMO.Application/ControloComparacao/` (`IControloComparacaoService`, models, validator), `src/DMO.Web/Endpoints/`, `src/DMO.Web/Pages/Controlo/`, `src/DMO.Web/Frontend/Controlo/`.

**CONTEXT PACK**
- Identity: NEW `comparacao_id` per event; subject key `(comparacao_id, cm_id)`; reuse existing `cm_id`.
- Decision vocabulary closed: exactly `Manter` / `Colocar de parte`.
- `Colocar de parte` requires non-blank justification.
- Confirmation gate: all measured subjects decided.
- Calculations use `PesoRowCalculationRules.ComputeRows` with the Peso's frozen facts.
- The original Peso (measurements, average, status, PDF, frozen facts) is never written.

**LEGACY / TRAPS TO IGNORE**
- **Do NOT use `ComparisonComposer` / `ComparisonCompositionTests`** — dead P2-T06 seam for the rejected `previous_peso_id` model. The new Comparação does not reference it.
- No `previous_peso_id` anywhere.

**DO NOT TOUCH**
- Comparação backend/domain/persistence/tests (already correct).
- Peso approve/reject/reopen logic.

**ACCEPTANCE CRITERIA**
- User can start a comparison on a decided Peso, add CMs, record measurements, decide each CM, confirm.
- Original Peso unchanged after the whole flow.
- `Colocar de parte` without justification is refused.

**FOCUSED TESTS**
- Existing: `ControloComparacaoServiceTests`, `ComparacaoRepositoryIntegrationTests`, `Migration009ControloComparacaoDomainTests` stay green.
- New: endpoint/UI integration tests for start/add/measure/decide/confirm; zero-effect assertion.

---

### STEP R-05 — Resumo / Folha de Controlo: reconcile read-projection vs persisted record  **[analysis + possible backend]**

**WHY THIS STEP EXISTS**
The corpus shows Resumo implemented as a **read projection only** (no `resumo` table). The Functional Manual §6.4 describes a Folha lifecycle (Rascunho → Submetida → Aprovada/Rejeitada) and persisted control evaluation. These must be reconciled before implementation.

**CURRENT STATE**
- Read-only Resumo entry surface exists (Razor page keyed by `jobon_id` traversal).
- No `resumo_id` table/column/route.
- Folha five-item OK/NOK states recorded as **not stored on Resumo** per delta table.

**FUNCTIONAL TARGET** (Manual §6.4)
- Five families: CM, BQ, MF, PU, CS.
- Per piece: OK/NOK, observation, MCaliper link.
- PU/CS from Job On context (not Armazém).
- Folha states: Rascunho → Submetida → Aprovada/Rejeitada; reopen path.
- Operador edits; Responsável decides.

**EXACT DELTA**
Determine whether the read projection satisfies the Manual, or whether a persisted Folha record (states, OK/NOK, observations, MCaliper links) is required. **If the Manual's Folha lifecycle is required, a persisted record + edit/submit/decide service is missing.**

**DEPENDENCIES**
R-01. Consumes Peso/Pegamentos/Job On context.

**FILES / AREAS TO INSPECT**
`src/DMO.Application/ControloCreate/IProductionResumoRead.cs`, the Resumo Razor page, `architecture/RECORD_LIFECYCLES.md` §7–§8 references in corpus.

**CONTEXT PACK**
- `resumo_id → jobon_id + applicable component contexts` (`cm_id`/`mf_id`/`bq_id`); no generic component ID.
- Folha ≠ Resumo (distinct concepts per delta table).
- Resumo PDF is derived; record ≠ PDF.
- PU/CS sourced from Job On context.

**LEGACY / TRAPS TO IGNORE**
- Do not add `peso_id` / `controlo_sheet_id` FK to Resumo (explicitly forbidden by R-08 scan rule).
- Do not store Folha OK/NOK on Resumo if the distinction must hold.

**DO NOT TOUCH**
- Peso records.

**ACCEPTANCE CRITERIA**
- Either the read projection is confirmed sufficient against the Manual, **or** a persisted Folha lifecycle is implemented with edit/submit/approve/reject and append-only history.

**FOCUSED TESTS**
- New: Folha state-transition tests; OK/NOK + observation + MCaliper persistence tests (if persisted path chosen).

---

### STEP R-06 — Documents / PDF generation + storage  **[backend + integration]**  **[independent]**

**WHY THIS STEP EXISTS**
Schema for the PDF directory exists (migration 004) but actual PDF generation, file storage, naming, and production-date usage are **not proven** in the corpus. P2-T08 plan listed these as expected/not-yet-existing.

**CURRENT STATE**
- `pdf_directory_settings` table exists.
- `AvailabilityState` shared component exists (P2-T01).
- No `src/DMO.Application/Documents/`, no Files/Pdf adapter proven.

**FUNCTIONAL TARGET** (Manual §8)
- Generate Peso PDF in Controlo_Create from the shared `PesoSheetReadModel`.
- Directory `<base>/<reference>/<production-number>/`; file `Peso_<reference>_<machine>.pdf`.
- "Data" field = Job On production date (never `SubmittedAt`).
- Never overwrite existing file (`already-available`).
- Structured record is truth; PDF is derived.

**EXACT DELTA**
Implement generation + availability + storage adapter; wire to the owning Peso record; resolve base directory from operator-configured setting.

**DEPENDENCIES**
Peso implementation (done). R-01 for gating.

**FILES / AREAS TO INSPECT**
`src/DMO.Application/Documents/` (to create), `src/DMO.Infrastructure/` (file/pdf adapter), `src/DMO.Web/Endpoints/`, `pdf_directory_settings` access.

**CONTEXT PACK**
- Document access gated by owning workflow permission — no separate document authorization model.
- Base directory resolved from `pdf_directory_settings`; never hardcoded.
- No local filesystem path printed inside the PDF.
- Path/filename never used as a join key.
- Availability states: `Available` / `NotGenerated` / `NotFound` / `WorkspaceUnavailable` / `Refused`.

**LEGACY / TRAPS TO IGNORE**
- Legacy browser/computer-local "reports directory" concept is superseded by server-side resolution.
- Do not create an artificial document identity.

**DO NOT TOUCH**
- Peso record writes.

**ACCEPTANCE CRITERIA**
- PDF generated at deterministic path with correct name and production date.
- Regeneration reports `already-available` without overwrite.
- Undecided/unanchored Peso refuses generation.

**FOCUSED TESTS**
- New: generation precondition tests, naming tests, no-overwrite test, production-date test.

---

### STEP R-07 — Email: group routing resolution + send  **[backend + integration]**  **[depends R-06]**

**WHY THIS STEP EXISTS**
Email-template group-routing schema exists (migration 009: `machine_group`, `email_list_id`) but the resolution algorithm and SMTP send are not proven.

**CURRENT STATE**
- `email_templates`, `email_lists`, `email_list_recipients` exist; group-routing columns exist.
- Send logic / transport adapter unproven.

**FUNCTIONAL TARGET** (Manual §8.8–8.10)
- B1/B2/B3 → Group B; C1/C2/C3 → Group C; other → `machine-group-unsupported`.
- Group → template → recipient list; no manual selection.
- Ambiguous templates → `email-template-ambiguous`; missing list → `email-group-not-configured`; empty list → `email-list-empty`.
- Send attaches existing PDF (never regenerates); missing PDF → `pdf-not-generated`.
- Transport failure → typed refusal; never alters Peso/decision/document.

**EXACT DELTA**
Implement routing resolution + send adapter + typed refusals.

**DEPENDENCIES**
R-06 (PDF must exist to attach).

**FILES / AREAS TO INSPECT**
`src/DMO.Application/Documents/` (or Communication area), email template/list repositories, transport adapter.

**CONTEXT PACK**
- Machine codes B1–C3 are independent; no "Linha B"/"Linha C" grouping beyond the B/C email group.
- Recipients resolved exclusively from configured lists; never hardcoded.
- No placeholder/substitution syntax in template text.
- Send evidence returned (file, template, recipients, group, timestamp); no persistent send table.

**LEGACY / TRAPS TO IGNORE**
- Do not invent manual recipient selection.

**DO NOT TOUCH**
- Email template settings UI (Controlo Definições, already done).

**ACCEPTANCE CRITERIA**
- Correct group resolved per machine; correct template + recipients; PDF attached; typed refusals on every failure path.

**FOCUSED TESTS**
- New: routing resolution tests (all machines, ambiguous, missing list, empty list), send-attach test, transport-failure test.

---

### STEP R-08 — Controlo repairer dead-surface cleanup  **[cleanup-only]**  **[independent]**

**WHY THIS STEP EXISTS**
Repairer *functional ownership* belongs to Boquilhas > Definições (Manual §7.3). The Controlo repairer service surface is dead code (zero callers). Shared error tokens prevent naive deletion.

**CURRENT STATE**
- Repairer tables physically in Controlo_Create schema (migration 004).
- Boquilhas Definições routes/page exist and serve repairers.
- Six Controlo repairer service methods + validator overloads + carriers present but caller-free.
- Three shared tokens (`NameRequired`, `RepairerNotFound`, `MachineUnknown`) consumed by Boquilhas.

**FUNCTIONAL TARGET** (Manual §7.3)
Repairer administration owned by Boquilhas > Definições; Controlo repairer surface obsolete.

**EXACT DELTA**
Remove dead Controlo repairer members **after** extracting the three shared tokens to a shared location so Boquilhas keeps working.

**DEPENDENCIES**
None functional; must coordinate token extraction before deletion.

**FILES / AREAS TO INSPECT**
`IControloDefinicoesService`, `ControloDefinicoesService`, `ControloDefinicoesValidator`, `ControloDefinicoesModels.cs`, `ControloDefinicoesValidationErrors`, Boquilhas Definições consumers.

**CONTEXT PACK**
- Do not delete shared tokens that Boquilhas consumes.
- Preserve machine-assignment and repairer data (shared tables stay).

**LEGACY / TRAPS TO IGNORE**
- `CONTROL_SETTINGS_REPAIRERS_EMAIL_PDF_DELTA.md` wording that placed repairers under Controlo is superseded for ownership.

**DO NOT TOUCH**
- Boquilhas Definições routes/page/service.
- Shared repairer/assignment tables.

**ACCEPTANCE CRITERIA**
- Controlo repairer dead members removed; Boquilhas repairer flows unaffected; shared tokens intact.

**FOCUSED TESTS**
- Existing: Boquilhas Definições tests stay green; Controlo repairer dead-surface verification.

---

## 5. DEPENDENCY GRAPH

```
R-01 (availability/routes verify)
   │
   ├──► R-02 (Pegamentos backend) ──► R-03 (Pegamentos UI)
   │
   ├──► R-04 (Comparação UI)                [independent]
   │
   ├──► R-05 (Resumo/Folha reconcile)       [independent]
   │
   ├──► R-06 (PDF generation/storage)  [independent]
   │        │
   │        └──► R-07 (email routing/send)
   │
   └──► R-08 (repairer dead-code cleanup)   [independent]
```

- **Sequential:** R-02 → R-03; R-06 → R-07.
- **Independent / parallelisable:** R-04, R-05, R-06, R-08 (all gated only by R-01).
- **Backend-only:** R-02, R-06, R-07, R-08.
- **Frontend-only:** R-03, R-04.
- **Analysis-first:** R-05.
- **Cleanup-only:** R-08.

---

## 6. DEAD / LEGACY SURFACES TO IGNORE

| Surface | Why dead | Do not |
|---|---|---|
| `ComparisonComposer` + `ComparisonCompositionTests` | Dormant P2-T06 seam for rejected `previous_peso_id` model | Wire, extend, or treat as Comparação |
| `previous_peso_id` concept | Never exists; identity tests prove absence | Introduce any previous-Peso relation |
| `production_id` / `job_on_revision_id` | Never created | Add these identities |
| Old Boquilhas `Início`/`Irreparável`/close-reopen/4-bucket | Superseded by migrations 007–008 | Re-add lifecycle or movement types |
| Controlo repairer service members | Zero callers; ownership moved to Boquilhas | Call or extend them |
| `BETA_MASTER_RECONCILIATION.md` "foundation phase" | Stale (predates migrations 003–010) | Use as current-state evidence |
| Old plans listing `CurrentBuildAvailable = []` as target | Foundation-era | Treat as current availability |
| HISTÓRICO GLOBAL (`historia`) | Deferred by design | Register route/availability |
| Legacy browser-local reports directory | Superseded by server-side resolution | Reintroduce client-side path authority |

---

## 7. ALREADY-CORRECT SURFACES TO PRESERVE

| Surface | Evidence | Preserve because |
|---|---|---|
| Canonical 13-module vocabulary + `ModuleCatalog` | `ModuleCatalog.cs`, P1-T04 | Enforcement boundary for all routes |
| Fail-closed `AccessResolver` + `ModuleAuthorizationHandler` | `DirectRouteEnforcementTests` | Security model |
| ADMIN/USER separation (no ADMIN super-user) | P1-T05 | Foundation rule |
| Tool master + `ToolRepository` | Migration 003, `FerramentasLightEndpoints` | Canonical `tool_id` registry |
| Job On snapshot invariant (new context ids; duplication reuses `tool_id`) | Migration 003, `JobOnRepository.ApplyKeepAsync` | Identity model |
| Peso identity + water/glass density + calculations | Migrations 004–006 | Core Controlo |
| Peso approve/reject/reopen on same `peso_id` | Migration 006, `peso_review_decisions` | Lifecycle model |
| Boquilhas 3-movement + replay + provisional anchor + association | Migrations 007–008 | Current Boquilhas model |
| Comparação backend/domain/persistence + tests | Migration 010 + full test suite | Already correct; UI is the only gap |
| Shared frontend components (`CommonState`, `RecordStatus`, `AvailabilityState`, `DenseDataTable`, `AuditTrail`) | P2-T01/T02 | Reuse, never rebuild |
| Admin Users/Templates + audit foundation | Migrations 001–002, Admin pages | Administration model |
| Controlo Definições (PDF dir, email lists/templates, glass density) | Migrations 004–005, 009 + endpoints | Settings ownership |

---

## 8. ITEMS THAT CANNOT BE PROVEN FROM THE PROVIDED CORPUS

1. **Exact current content of `ModuleRegistrations.CurrentBuildAvailable`.** Historical docs show `[]`; per-workstream registration implies it was populated as surfaces landed, but the current literal list is not present in the provided excerpts. → R-01 must verify.

2. **Whether Comparação/Pegamentos/Resumo are registered as reachable destinations** (distinct from schema/backend existing). Tied to item 1.

3. **Completeness of the PDF generation and email send adapters.** Schema and plans exist; the actual `Documents`/`Files`/`Pdf` application + infrastructure code is not visible in the provided excerpts. → R-06/R-07 must confirm-or-build.

4. **Folha de Controlo persisted lifecycle vs read-projection sufficiency.** The corpus proves a read projection exists and a persisted `resumo_id` record is an unimplemented remainder, but whether the Manual's Folha states require persistence is an interpretation the Functional Manual plus `RECORD_LIFECYCLES.md` must settle. → R-05 analysis.

5. **Exact current UI page inventory for Controlo Create/Approve and Boquilhas** beyond the endpoints/pages referenced. The corpus references several but not a complete page list.

6. **Whether the shared repairer error-token extraction (R-08 prerequisite) has any hidden additional consumers** beyond Boquilhas. A caller scan is required during R-08.

> Where the Functional Manual and repository together give a clear answer (Comparação UI absent, Pegamentos absent, backend otherwise present), no owner decision is needed. The items above are **verification** tasks, not product decisions.