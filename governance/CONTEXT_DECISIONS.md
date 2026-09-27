# Context Decisions

Record of the corrections applied to this context pack, so the structure is traceable rather than
implicit. This is a decision record, not functional authority.

## D-01 — Organize the pack by module

**Decision.** The unit of organization is the module: `modules/<module>/FUNCTIONAL.md` (rules) and
`modules/<module>/IMPLEMENTATION_STATE.md` (evidence). Cross-module rules live in `shared/`.

**Rationale.** The brief requires each module to concentrate the information needed to understand it
(behavior, rules, flows, relations, decisions, maps), and forbids organizing by date, history, origin,
author, or tool. A single flat monolith plus loose maps did not expose which rule belongs to which
module.

## D-02 — Resolve the access contradiction in favor of the template canon

**Decision.** `Utilizador → Template de acesso → Módulos/permissões` is the access model. Fixed
business profiles `Operador / Controlador` and `Responsável` are classified LEGACY CONTAMINATION
and removed from active functional documents. Module behavior that used to be described through those
names is now expressed as module actions (for example Controlo create versus Controlo approve).

**Rationale.** Two documents contradicted each other at the top of the authority chain: the README
access canon (newest owner-confirmed clarifications) and the functional manual's "three functional
profiles" section, duplicated by the module-flow profile table. The implementation evidence names the
Controlo Create and Controlo Approve application areas, which supports describing capabilities as
actions rather than roles. Keeping both statements produced two sources of truth for one question.

## D-03 — Do not invent the permission catalog

**Decision.** The permission leaf of the canon is marked **UNKNOWN** in
`shared/ACCESS_MODEL.md`: exact identifiers and granularity are code-owned in DMO-MODULAR and are
not reproduced here.

**Rationale.** The canon claims "modules/permissions", but no in-repo document enumerated any
permission. Inventing identifiers or mapping legacy profile names to permissions would recreate the
contamination the canon forbids.

## D-04 — Split the functional manual; no monolith

**Decision.** `BETA_FUNCTIONAL_MANUAL.md` was split into the module functional documents plus the
shared functional documents. The Authority section of `README.md` now names that set.

**Rationale.** A single monolith conflicts with module-first organization and mixes scope, access,
identity, module rules, documents, and boundaries. No functional rule was dropped; the previous file
remains in git at commit `e56ef46`.

## D-05 — Split implementation evidence per module

**Decision.** `CURRENT_STATE.md` was split into `modules/*/IMPLEMENTATION_STATE.md`, with
cross-module evidence in `shared/ACCESS_MODEL.md` and `shared/DOCUMENTS_AND_EMAIL.md`. Global
readiness is captured in `delivery/ROADMAP.md` and the open unknowns of `INDEX.md`.

**Rationale.** Evidence about a module belongs with that module. The old single file mixed module
evidence with global build-availability evidence.

## D-06 — "DONE" is not a delivery status

**Decision.** The roadmap no longer uses `# DONE`. It uses "Verified evidence — NOT a delivery
status", and every item points to the module state document that qualifies it.

**Rationale.** The previous DONE list equated "evidence found" with "implemented", while the
evidence file marked Comparison UI, Pegamentos, and Admin Applications/Audit as partial or not
proven.

## D-07 — Quarantine the archived raw plan; delete the empty stubs

**Decision.** The raw Qwen reconstruction roadmap moved to
`archive/qwen/` with a quarantine banner. The two committed Qwen tool-error stubs were deleted.

**Rationale.** The raw roadmap reads like a live plan, cites files that do not exist here, and uses
legacy role language. The two stubs contain no evidence and falsely imply that a Pegamentos
reconciliation and a second evidence pass were produced.

## D-08 — Archive the audit with a resolution banner

**Decision.** `AUDIT_REPORT_CONTEXT_INTEGRITY.md` moved to `archive/` with a banner stating it is
historical and that its findings are resolved or tracked by the current pack.

**Rationale.** The audit was a read-only snapshot of the previous structure. Left at the repository
root, its "Critical contradiction" would remain an active, unresolved claim about documents that no
longer exist in that form.

## D-09 — Previous pack preserved by git reference

**Decision.** Removed documents are not duplicated under `archive/`; they are referenced by commit
`e56ef46` in `archive/README.md` and `INDEX.md`.

**Rationale.** Traceability without creating two active-looking copies of superseded rules.

## D-10 — Distinguish the Boquilhas register identity from the frozen BQ context

**Decision.** `shared/IDENTITY_AND_RELATIONS.md` states explicitly that the Boquilhas register
keeps its own register identity when the provisional `tool_id` anchor is replaced by the matching
`bq_id`, and that this is not the frozen Job On context `bq_id`.

**Rationale.** The functional manual used both `boquilhas_id` and `bq_id`; without a note they
read as the same thing.

## D-11 — Mark positional pairing as UNKNOWN instead of importing it

**Decision.** The old rule that pairs water readings by their position in the reading table is
recorded as UNKNOWN in `modules/controlo/FUNCTIONAL.md` and in the legacy registry. It was not
copied into the current Peso rules.

**Rationale.** It appears in the deleted full-scope manual but not in the clean Beta manual. Copying
it would import a rule the current authority does not state; dropping it silently would hide a real
open question.

## D-12 — External evidence stays external

**Decision.** References to DMO-MODULAR source paths are kept, always marked as external, not present
in this repository, and not pinned to a commit.

**Rationale.** The pack is not self-contained; every evidence claim resolves into an external
repository. Marking the dependency prevents silent drift from being read as verified fact.

## D-13 — Canonical vocabulary is preserved

**Decision.** Module names, the canonical identities, the closed Boquilhas movement vocabulary, the
Comparison-as-Peso-workflow rule, and the record-versus-PDF rule were preserved unchanged in meaning.

**Rationale.** The pack's module vocabulary and identity graph were coherent; the correction targets
structure, access terminology, and non-authority material, not the model itself.
