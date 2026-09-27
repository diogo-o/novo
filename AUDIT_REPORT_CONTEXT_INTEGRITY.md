# DMO Novo Repository — Context Integrity Audit

Audit scope: `diogo-o/novo`, branch `main`, HEAD `7bc8ddd`.
This is a **read-only** audit. No repository file was modified and no recommendation below was performed.

Repository inventory (11 tracked files): `README.md`, `BETA_FUNCTIONAL_MANUAL.md`,
`CURRENT_STATE.md`, `ROADMAP.md`, `FRONTEND_RULES.md`, `maps/IDENTITY_RELATIONS.md`,
`maps/MODULE_FLOW.md`, `maps/DOCUMENT_FLOW.md`,
plus `qwen/DMO_BETA_IMPLEMENTATION_STATE_RECONSTRUCTION_ROADMAP_RAW.md`,
`qwen/DMO_BETA_SECOND_PASS_EVIDENCE_DEEPENING_RAW.md`,
`qwen/PEGAMENTOS_RECONCILIATION_FOCUSED_EVIDENCE_RAW.md`.

---

# Summary

The repository is a deliberately small "Beta-only context pack" whose top-level documents
(`README.md`, `BETA_FUNCTIONAL_MANUAL.md`, `CURRENT_STATE.md`, `maps/*`, `FRONTEND_RULES.md`) are
internally coherent on **module boundaries, identity graph, documents, and excluded domains**. The
core functional model (Job On / Controlo / Ferramentas / Boquilhas / Admin, canonical identities
`tool_id`/`jobon_id`/`cm_id`/`mf_id`/`bq_id`, closed Boquilhas movement vocabulary, Comparison as a
Peso workflow, PDF-derived-from-record) is stated consistently and would not by itself mislead an
implementer.

However the pack has **two self-contradictions that sit at the very top of the authority chain** and
will split an implementation agent's attention:

1. The most recent commit (`7bc8ddd`) inserted an "Identity and Access Canon" in `README.md` that
   states the access model is `Utilizador → Template de acesso → Módulos/permissões` and that
   `Operador`/`Responsável` "belong to previous models and must not be reintroduced." The functional
   manual — declared by the same README as *the* functional authority — still defines exactly three
   **functional profiles** named `Admin`, `Operador / Controlador`, and `Responsável`, and
   `maps/MODULE_FLOW.md` still carries a "Profile Variants" table keyed on those same labels.

2. No document specifies permission-level granularity (e.g. `controlo_criar`, `controlo_aprovar`).
   The create→submit→approve distinction is represented **through profile names**
   (`Operador` submits, `Responsável` approves) rather than through module permissions — which is the
   exact "procedural role-as-architecture" pattern the audit is meant to catch.

Additional contamination is concentrated in `qwen/`: two of the three "raw evidence" files contain
only a Qwen tool-error string (empty stubs), while the third is a detailed implementation
reconstruction (migration tables, R-01…R-08 steps) that references irrecoverable external-repository
files and reads like a live plan despite being archived raw input.

Net assessment: **the pack is not safe to hand to a coding agent as-is.** The contradiction between
the access canon and the profiles model is Critical; the empty evidence stubs and the raw-roadmap
confusion are High.

---

# Critical issues

### C-1 — README access canon contradicts the functional manual on Operador/Responsável

- `README.md` (lines 27–33, added in HEAD commit `7bc8ddd`) says the model is
  `Utilizador → Template de acesso → Módulos/permissões` and: *"Do not interpret the application as
  being based on fixed business roles such as `Operador` or `Responsável`. Those names belong to
  previous models and must not be reintroduced as the current architecture."*
- `BETA_FUNCTIONAL_MANUAL.md` §2 (lines 23–55) — the declared functional authority — states *"There
  are exactly three functional profiles: 1. Admin, 2. Operador / Controlador, 3. Responsável"* and
  gives a per-module behavior table whose columns are `Operador / Controlador` and `Responsável`.
- `maps/MODULE_FLOW.md` (lines 41–49) repeats a "Profile Variants" table using the same labels.

Impact: an agent cannot tell whether the current architecture *has* an Operador/Responsável profile
dimension. It will either strip the profile-experience behavior (breaking approval workflows:
"Responsável decides", "Responsável selects tooling") or reintroduce fixed business roles as the
access mechanism. This is the highest-leverage misunderstanding in the repo.

Root cause (classification, not a fix): the manual itself already separates "profile" (how a person
works inside an assigned module) from "access template" (which modules are assigned), which is
*compatible* with the README's template-based access claim. The README's added sentence over-reaches
by forbidding the *names* themselves, colliding with the manual's retained profile vocabulary. The
two documents are now two sources of truth for the same question.

### C-2 — Permission granularity is claimed but never specified

- The README canon ends in "Módulos/**permissões**", and `CURRENT_STATE.md` reports a
  "code-owned module catalog and server-side authorization abstractions" (`ModuleCatalog.cs`,
  `ModuleRegistrations.cs`). But **no canonical document in this pack enumerates any permission**
  below the module level.
- The `Controlo` create/approve split — which the task calls out as the expected shape of
  `controlo_criar`, `controlo_aprovar` — is expressed in the manual purely as
  *"The Operador / Controlador prepares … and submits"* vs *"The Responsável reviews, approves,
  rejects."* (`BETA_FUNCTIONAL_MANUAL.md` §6, lines 148–150) and in `MODULE_FLOW.md` lines 47–49.

Impact: an implementer asked to build the access layer will either invent a permission model the
docs never define, or encode `Operador`/`Responsável` as fixed roles — recreating exactly the
legacy coupling the README warns against. The "permissions" leaf of the canon has no authoritative
definition in-repo.

---

# Legacy contamination

### L-1 — `qwen/` raw reconstruction reads as a live plan (High)

`qwen/DMO_BETA_IMPLEMENTATION_STATE_RECONSTRUCTION_ROADMAP_RAW.md` (567 lines) is a full
implementation roadmap (migration chain 001–010, steps R-01…R-08, "DO NOT TOUCH", "ACCEPTANCE
CRITERIA", "LEGACY / TRAPS TO IGNORE"). It is archived under `qwen/` and is marked "RAW", but:

- nothing at the top of the file declares it historical/superseded relative to the active
  `ROADMAP.md` (which is now nearly empty);
- it references `reports/BETA_MASTER_RECONCILIATION.md` and
  `architecture/RECORD_LIFECYCLES.md` — files that **do not exist in this repository**;
- it uses role language (line 322: *"Operador edits; Responsável decides"*).

A new agent that globs for "roadmap" will find this file and may code against DMO-MODULAR migration
numbers and file paths as if they were this repo's own, and may resurrect the stale
`BETA_MASTER_RECONCILIATION.md` "foundation phase" conclusion that the same file itself labels STALE.

### L-2 — Two of three `qwen/` evidence files are empty error stubs (High)

`qwen/DMO_BETA_SECOND_PASS_EVIDENCE_DEEPENING_RAW.md` and
`qwen/PEGAMENTOS_RECONCILIATION_FOCUSED_EVIDENCE_RAW.md` each contain a single placeholder line:

> "The requested file reference is not currently visible. Use files.search or files.list to
> rediscover the file, then retry with a returned ref_id or file_id."

These are committed Qwen tool-error messages, not evidence. They are misleading in two directions:
the filenames imply "Pegamentos reconciliation" and "second-pass evidence deepening" have been
produced and captured, but the content is absent — an agent will either assume that reconciliation
already happened (and not re-derive it) or treat a tool-error string as a meaningful instruction.

### L-3 — Deleted `MANUAL.md` still reachable in git history (Medium)

The original Portuguese full-scope manual (`MANUAL.md`) — covering the **excluded** domains Armazém,
Reparação Interna, Reparação Externa, Tampões — was deleted in commit `a465195` and is now absent
from the working tree, but persists in history (commits `84ce791`, `a465195`). An agent using
`git log`/`git show` for context can pull back the excluded-domain module model and the detailed
profile definitions (`§1.1`), reintroducing superseded scope into Beta reasoning. It is legacy and
superseded, but not obviously quarantined.

### L-4 — `ROADMAP.md` "DONE" conflates "evidence found" with "implemented" (Medium)

`ROADMAP.md` lists items under `# DONE` such as *"Job On service evidence for …"* and *"Comparison
backend/domain/persistence evidence …"*. `CURRENT_STATE.md` clarifies that several of these are
PARTIAL or NOT PROVEN (Comparison UI unwired, Pegamentos entirely unproven, Admin Applications/Audit
unproven). The DONE list carries none of that nuance, so an agent may conclude the Comparison and
Pegamentos areas are finished.

---

# Role vs Permission issues

The following are all occurrences where `Operador` / `Responsável` (or the concept of a fixed
business role/title) touches the architecture, and whether each is consistent with the README canon.

| Location | Occurrence | Consistency risk |
|---|---|---|
| `README.md` §Identity and Access Canon (L27–33) | Declares Operador/Responsável are *previous models*; access is template→modules/permissions. | This is the newest statement but conflicts with the manual (C-1). |
| `BETA_FUNCTIONAL_MANUAL.md` §2 (L23–55) | "Exactly three functional profiles": Admin, Operador / Controlador, Responsável; per-module behavior table. | Manual treats profiles as *experience*, distinct from access; but uses the same names README forbids. |
| `BETA_FUNCTIONAL_MANUAL.md` §3–§7 (L106–109, L148–150, L184, L199, L279) | Responsável selects tooling/approves; Operador confirms/measures/submits; Boquilhas same actions for both. | Role names carry create/approve semantics = role-as-permission. |
| `maps/MODULE_FLOW.md` §Profile Variants (L41–49) | Table keyed on Operador/Controlador vs Responsável vs Admin. | Duplicates the manual table; now also contradicts README canon. |
| `qwen/…ROADMAP_RAW.md` R-05 (L322) | "Operador edits; Responsável decides." | Raw archived role language; contaminates Resumo/Folha reconciliation. |
| Deleted `MANUAL.md` §1.1 (git history) | Detailed Operador/Controlador/Responsável profile definitions + "titles never grant access." | Historical; reachable via history; includes excluded-domain model. |

Core architectural finding: **the pack does not represent module permissions as the leaf of the
access model.** The create/approve distinction inside `Controlo` is modeled as *profile identity*
(`Operador` submits, `Responsável` approves) rather than as *permissions*
(`controlo_criar`, `controlo_aprovar`). `CURRENT_STATE.md` and the raw roadmap both point to a
"13 canonical module" catalog in DMO-MODULAR, but that catalog is an external artifact and its
permission-level granularity is not reproduced here. There is no in-repo place where a distinct
"same module, different action" authorization is expressed as a permission. This is the exact
"user titles instead of permissions" anti-pattern the audit targets.

---

# External repository coupling

| Reference | Where | Classification | Risk |
|---|---|---|---|
| `DMO-MODULAR` | `README.md` L10/L48, `CURRENT_STATE.md` (header + 11 path references in "Evidence References" L73–83) | Authority/evidence dependency | The pack explicitly leans on DMO-MODULAR source as *evidence of what exists*. It is declared "not functional authority," which is the correct framing, but the pack is not self-contained: every "evidence" claim resolves into a repo that is not present here. If DMO-MODULAR advances, the evidence silently drifts (this already happened with the stale reconciliation). **Justified as evidence; risky as authority.** |
| `reports/BETA_MASTER_RECONCILIATION.md` | `qwen/…ROADMAP_RAW.md` L29, L76, L527 | Historical reference | Self-declared STALE in the same file. An agent that finds this path will chase a file not in this repo and may resurrect the "foundation phase" conclusion. **Historical; must not be followed.** |
| `architecture/RECORD_LIFECYCLES.md` | `qwen/…ROADMAP_RAW.md` L331 | Historical reference | Points to a sibling doc absent from this repo. Referenced for Resumo/Folha lifecycle reconciliation; not resolvable here. |
| `dmo-beta-master`, `dmo-master`, `workbench`, `dmo-work`, `DMO-MODULAR` as code checkout | — | **Absent** | No reference to any of these repository names exists in the current working tree. The only external repository name present is `DMO-MODULAR`. (Searched the full tree for `dmo-beta-master`, `dmo-master`, `workbench`, `dmo-work`, `DMO-MODULAR`.) |
| `github.com/diogo-o/novo.git` | git remote `origin` | This repo itself | Not a coupling; listed for completeness. |

Note on implicit authority: the raw roadmap's own "Authority order" (functional manual → repository
corpus → plans/reports) relies on a **corpus that lives in DMO-MODULAR, not here**. So the pack's
stated evidence chain depends on an out-of-repo codebase that can only be inspected by an agent with
CLI/network access to that checkout, and whose state is not pinned or versioned in this repo.

---

# Module permission model (focus area 5)

- The pack's module vocabulary is healthy and consistent: `Job On`, `Controlo` (with internal areas
  Peso / Pegamentos / Resumo / Comparison / Histórico), `Ferramentas`, `Boquilhas`, `Admin`. Internal
  Controlo areas are explicitly *not* modules (`BETA_FUNCTIONAL_MANUAL.md` §6 and §10;
  `README.md`), and Comparison is a workflow *inside* Peso. This is correct and not a source of
  confusion.
- The **broken** part is below the module level. The audit's expected shape —
  `Controlo: controlo_criar, controlo_aprovar` — is nowhere specified. The create/approve distinction
  is instead carried by profile names `Operador`/`Responsável` (`BETA_FUNCTIONAL_MANUAL.md` §2 and
  §6; `MODULE_FLOW.md` §Profile Variants). Boquilhas correctly avoids this by stating both profiles
  share the same operational actions (a place where a permission split is *not* needed).
- Consequence: a permission-driven refactor cannot be started from this pack, because the mapping
  from profile → permissions is undefined, and the one document that *would* define the permission
  leaf (`ModuleCatalog.cs`, cited in `CURRENT_STATE.md`) is external and not reproduced here.

---

# Agent safety

If a new coding agent receives this repository today, the likely misunderstandings are:

1. **It cannot resolve the role model.** It will read the README's "no fixed roles" canon, then the
   manual's three functional profiles, and has no tie-breaker. The most dangerous outcome is
   either dropping the Operador/Responsável experiences (losing approve/reject and
   tooling-selection gating) or hard-coding them as fixed roles (violating the canon).
2. **It will invent or resurrect a permission model** because `controlo_criar`/`controlo_aprovar`-
   style permissions are claimed but never defined in-repo.
3. **It will chase DMO-MODULAR files** (and the stale `BETA_MASTER_RECONCILIATION.md`) as authority,
   because all "evidence" citations resolve into that external repo, and treat the raw roadmap's
   migration numbers and `R-01…R-08` steps as this repo's plan.
4. **It will assume Pegamentos/Comparison status is wrong.** The DONE list implies completion; the
   empty `qwen/PEGAMENTOS_RECONCILIATION_…` stub implies reconciliation happened; `CURRENT_STATE.md`
   says Pegamentos is unproven and Comparison lacks UI. Three documents give three signals.
5. **It may treat the two empty qwen stubs as content** (the committed Qwen tool-error string) or,
   conversely, assume the "deepening/reconciliation" evidence was lost and halt to re-derive it.

---

# Recommended cleanup actions

*(Recommendations only — none were performed in this audit.)*

1. **Reconcile the access canon with the manual.** Choose and state the single correct relationship
   between profiles and the access template/permission model, then align `README.md`,
   `BETA_FUNCTIONAL_MANUAL.md` §2, and `maps/MODULE_FLOW.md`. If profiles must exist as *experience*
   (not access), rewrite the README sentence to forbid fixed-roles-as-access without forbidding the
   profile vocabulary.
2. **Define the permission leaf.** Either enumerate permissions (e.g. `controlo_criar`,
   `controlo_aprovar`) and map profiles→permissions, or explicitly declare that create/approve is
   profile-driven architecture and stop claiming a permissions leaf in the canon.
3. **Quarantine `qwen/`.** Add a top-level marker (or move under a clearly-labelled `archive/`) that
   states these are superseded raw inputs, not live authority; strip or de-emphasize directive
   content (R-01…R-08, ACCEPTANCE CRITERIA).
4. **Remove or clearly mark the two empty qwen stubs.** They are Qwen tool-error text, not evidence;
   as present they falsely imply Pegamentos reconciliation and a "second pass" exist.
5. **Cut or pin external-repo citations.** Either inline the relevant DMO-MODULAR facts into
   `CURRENT_STATE.md` (self-contained evidence) or pin a commit/version so the evidence cannot drift
   silently; remove the dead `reports/BETA_MASTER_RECONCILIATION.md` and
   `architecture/RECORD_LIFECYCLES.md` references or mark them unresolvable.
6. **Align `ROADMAP.md` DONE with `CURRENT_STATE.md`.** Distinguish "evidence verified" from
   "implemented/shipped" so DONE does not imply the frontend/UI areas are finished.
7. **Mark the deleted `MANUAL.md` as superseded/historical** for any future agent that inspects git
   history (e.g. a `CHANGELOG`/`ARCHIVE` note), or prune the history if the excluded-domain model
   must never resurface.

---

# Priority

| Priority | ID | Finding |
|---|---|---|
| **Critical** | C-1 | README access canon vs functional-manual profile model contradiction (Operador/Responsável). |
| **Critical** | C-2 | Permission granularity claimed but never specified; create/approve expressed through role names. |
| **High** | L-2 | Two empty `qwen/` evidence stubs (committed tool-error text) falsely imply reconciliation. |
| **High** | L-1 | `qwen/…ROADMAP_RAW.md` reads as a live plan and cites external files that don't exist here. |
| **High** | — | External authority staleness risk via unpinned DMO-MODULAR evidence + stale `BETA_MASTER_RECONCILIATION.md`. |
| **Medium** | L-4 | `ROADMAP.md` DONE conflates evidence with implementation. |
| **Medium** | L-3 | Deleted `MANUAL.md` (excluded-domain, role definitions) reachable via git history. |
| **Low** | — | `ROADMAP.md` NOW/NEXT/LATER placeholders carry no priority and could be read as stale. |

*End of report. No repository file other than this report was created or modified.*