# Legacy Contamination Registry

Negative authority: concepts that belonged to previous models and **must not** be reintroduced as
current architecture, navigation, roadmap items, data, or acceptance tests.

This document is evidence of what to avoid, not a functional rule. It exists because several of these
concepts look plausible and reappear easily.

## 1. Fixed business profiles used as access architecture

| Concept | Class | Why it is not current | Do instead |
|---|---|---|---|
| `Operador` / `Controlador` as an access or experience dimension | LEGACY CONTAMINATION | The access model is `Utilizador → Template de acesso → Módulos/permissões`. There is no fixed business-role dimension. | Grant module actions through access templates. See `shared/ACCESS_MODEL.md`. |
| `Responsável` as an access or experience dimension | LEGACY CONTAMINATION | Same as above. | Use the granted approve/configuration actions. |
| "Profile Variants" tables that map module behavior to profile names | LEGACY CONTAMINATION | They encode roles as permissions. The current shape is Controlo Create versus Controlo Approve, i.e. actions inside one module. | Describe capabilities as module actions (`shared/MODULE_FLOW.md`, section 4). |
| "Admin profile behaves like…" as an operational variant | LEGACY CONTAMINATION | Admin is an assignable system module, not an operational production role. | Keep Admin as a module; never grant operational access implicitly. |
| Job titles such as "Chefe", "Engenheiro", "Metrologia" as permissions or as a separate profile | LEGACY CONTAMINATION | Titles are visual text only. | Never derive access or a variant from a title. |

Where the legacy labels are still needed for readability inside `archive/`, they remain archived
labels only and are not architecture.

## 2. Superseded architecture and data concepts

| Concept | Class | Why | Do instead |
|---|---|---|---|
| `previous_peso_id` | LEGACY CONTAMINATION | Never exists; Comparison reuses the existing `cm_id` and has no previous-production relation. | Continue the existing relation. |
| `production_id` / a second context layer | LEGACY CONTAMINATION | Never created; Job On already owns the production occurrence. | Use `jobon_id` plus frozen `cm_id`/`mf_id`/`bq_id`. |
| `ComparisonComposer` / `ComparisonCompositionTests` | LEGACY CONTAMINATION (dead code) | Dormant seam for the rejected previous-Peso model. | Do not wire, call, or extend it. |
| Old Boquilhas lifecycle: normal lifecycle, `Início`, `Irreparável`, close/reopen, four-bucket model | SUPERSEDED | Replaced by the closed three-movement model and derived outstanding quantity. | Use `Saída`, `Entrada`, `Entrada sem reparação`; derive outstanding at read time. |
| Controlo as repairer owner / Controlo repairer service surface | SUPERSEDED | Functional ownership of repairers is Boquilhas settings; the Controlo surface is caller-free. | Own repairer administration in Boquilhas. |
| Browser/computer-local "reports directory" or client-side document path authority | SUPERSEDED | Storage is server-side and deterministic. | Use the configured server-host base directory. |
| Peso reading pairing by table/row position | UNKNOWN | Older material described it; the clean Beta manual does not restate it, and it must not be confused with Comparison's explicit CM association. | Leave open; do not infer or implement. |
| `CurrentBuildAvailable = []` treated as the target state | SUPERSEDED | That was the foundation-era state. | Verify the current registration; see `delivery/ROADMAP.md`. |

## 3. Modules outside the current scope

These must not appear in Beta navigation, routes, templates, migrations, roadmap items, or acceptance
tests, not even through aliases or "temporary" screens:

- Armazém (physical stock);
- Reparação Interna (internal repair);
- Reparação Externa (external repair);
- Tampões (plug);
- Design Laboratório;
- a global História destination (deferred by design; local module history is different).

A **TP/Tampão field** in a Job On production configuration is a production field, not the excluded
Tampões module. Do not confuse them.

## 4. Evidence and authority contamination

| Item | Class | Why | Do instead |
|---|---|---|---|
| `MANUAL.md` (deleted in `a465195`, in git history only) | LEGACY CONTAMINATION | Full-scope manual: includes excluded domains and the old profile definitions. | Never read it as current. Use the module documents in this pack. |
| `reports/BETA_MASTER_RECONCILIATION.md` | REFERENCE ONLY (unresolvable, stale) | Self-declared stale; the file is not in this repository. | Do not chase it; it is not current-state evidence. |
| `architecture/RECORD_LIFECYCLES.md` | REFERENCE ONLY (unresolvable) | Referenced by archived raw material; file not in this repository. | Record the open question instead; do not cite it as authority. |
| Archived raw Qwen reconstruction roadmap (R-01…R-08) | REFERENCE ONLY | Raw external planning material; reads like a live plan but is not this repository's plan or roadmap. | Use `delivery/ROADMAP.md`; treat the raw file as non-authority. |
| Two committed Qwen tool-error stubs ("file reference is not currently visible") | LEGACY CONTAMINATION (removed) | They contain no evidence and falsely imply that a reconciliation or second pass was produced. | Treat Pegamentos and second-pass reconciliation as **not produced**. |
| `ROADMAP.md` "DONE" list (previous pack) | SUPERSEDED | It presented implementation evidence as delivered work. | Use "Verified evidence — NOT a delivery status" in `delivery/ROADMAP.md`. |
| Unpinned external DMO-MODULAR citations | REFERENCE ONLY | Evidence can drift silently when the external repository advances. | Keep the framing "evidence, not authority"; pin a commit when updating. |

## 5. Classification summary

| Class | Meaning |
|---|---|
| ACTIVE | Represents the current model; may be authority or current evidence. |
| SUPERSEDED | Replaced by later decisions; kept only for traceability. |
| LEGACY CONTAMINATION | Belongs to an older model and can induce wrong implementation. Do not reintroduce. |
| REFERENCE ONLY | Must not be in the active context; kept for traceability or because it is external and unresolved. |
| UNKNOWN | Not enough decision or evidence to resolve. Leave open. |
