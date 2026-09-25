# BA DMO — Beta Functional Manual

This is the functional authority for the Beta context. It describes user intent and business rules,
not a database design or a UI implementation. The Beta context is deliberately smaller than the
source material from which it was extracted.

## 1. Scope and Authority

Beta includes:

- Job On;
- Ferramentas;
- Controlo, with Peso, Pegamentos, Resumo, Comparison, and Histórico as internal areas/workflows;
- Boquilhas;
- Admin;
- structured documents, PDF generation/storage, and explicit PDF email sending;
- shared identity, access, navigation, audit, and design-system behavior.

Beta does not include the physical-stock, repair, plug, or design-laboratory domains. Those areas
must not be recreated through aliases, hidden routes, or “temporary” screens. TP/Tampão as a
production-configuration field in Job On is a field, not a Beta module.

## 2. Profiles, Access, and Context

There are exactly three functional profiles:

1. Admin
2. Operador / Controlador
3. Responsável

Profile and access are separate:

- the profile controls how a person works inside an assigned module;
- an access template controls which modules are available to that person;
- a job title is visual text only and never grants access;
- an Admin profile does not grant operational access automatically.

Effective access is resolved server-side from the canonical module catalog and the user’s assigned
template. An unassigned module is absent from normal navigation and denied on direct access. Hiding
a link or button is never an authorization mechanism.

Operational users land on Job On. An Admin-only user lands on Admin. There is no profile selector at
login. A missing active identity, missing grant, invalid command, or stale version fails closed.

The Beta profile/module behavior is:

| Module | Admin | Operador / Controlador | Responsável |
|---|---|---|---|
| Job On | No implicit operational access | Consult and confirm checks | Create, edit, duplicate, select tooling |
| Controlo | No implicit operational access | Measure, record, submit | Review, approve, reject, reopen |
| Ferramentas | No implicit operational access | Consult where granted | Maintain master data and rules |
| Boquilhas | No implicit operational access | Operational actions | Same operational actions |
| Admin | Users/access administration | No | No |

Boquilhas is deliberately the same operational workflow for Operador / Controlador and Responsável.

## 3. Canonical Identity and Ownership

- `tool_id` is the stable, invisible identity of a canonical Tool master record.
- `jobon_id` is the stable identity of one Job On production occurrence.
- `cm_id`, `mf_id`, and `bq_id` are frozen Tool contexts selected for that Job On occurrence/revision.
- Visible attributes such as family, reference, lot, and machine/line are for search, filtering, and
  confirmation. They are not a substitute for stable identity.
- A downstream module continues an existing relation. It does not recreate production or pair
  visible fields again to guess a relation.
- CM/MF preserve both Tool/context identity and the individual piece number where the workflow needs
  a piece number. BQ repair movements are quantity-based.
- Every new Job On, including duplication, creates NEW `cm_id`, `mf_id`, and `bq_id` context
  snapshots from the CURRENT canonical Tool rows. The canonical `tool_id` may be reused; prior
  context IDs are never reused and previous contexts remain frozen.
- `peso_id` belongs to Controlo. In production it continues the existing `cm_id` relation; a
  pending pre-JobOn Peso may temporarily anchor to the canonical `tool_id` until a human-confirmed
  association to the real `cm_id` is possible.
- Each Comparação occurrence gets a new `comparacao_id` and reuses the existing `cm_id`.
- A Boquilhas register keeps the same `boquilhas_id` when its provisional `tool_id` anchor is
  later replaced by the matching real `bq_id`.
- There is no `production_id`, `previous_peso_id`, or parallel identity used to recreate an
  already-existing relation.

## 4. Job On

Job On is the production and planning hub. It owns the production occurrence, its revisions, selected
Tool contexts, production-specific configuration, checks, history, and production documents.

### 4.1 Planning and selection

- The calendar locates planned productions and their Job Ons.
- A click selects a day; an explicit action opens or creates the Job On. Changing month does not
  auto-select a day.
- Editing the production date updates the same Job On identity; it does not create a duplicate.
- A saved edit creates a new revision while preserving prior snapshots.
- Colors identify machine/line, not a hidden business state.

### 4.2 Production data

The Job On stores the exact production/reference context, selected CM/MF/BQ plus lot, and production-
specific fields such as FF, Calibres, Pinças, PU, CS, and TP/Tampão where applicable. PU, CS, and
TP/Tampão are manually configured production fields. A duplicated Job On copies them as editable
starting values; they are not permanent defaults.

Job On does not own Tool master data, control results, or repair history. Later master edits must not
silently rewrite a historical production snapshot.

### 4.3 Tooling and roles

- The Responsável selects CM/MF/BQ from options filtered by reference and machine/line.
- Filtering narrows options; it does not infer compatibility or choose a Tool.
- The Operador reads the Job On and confirms applicable checks.
- The Responsável edits production configuration and tooling.
- A check rule belongs to the Tool lot; Job On materializes occurrences for this production.
- Check confirmation is manual and records authenticated user and timestamp. It is never inferred
  from stock, repair, technical condition, usage, or elapsed time.
- Duplicating a Job On does not copy old check history; the new occurrence gets its own checks.
- Duplication creates a NEW `jobon_id`. The source contributes canonical `tool_id` selections,
  but the duplicate creates NEW CM/MF/BQ context IDs from the CURRENT Tool master state rather than
  reusing or copying the source context IDs.
- The source Job On and its frozen contexts remain unchanged.

### 4.4 Downstream context

Controlo consumes the exact Job On context and has no independent Job On selector. A control record
may inspect a different valid lot without changing the Job On plan or selected production tooling.
The same frozen context is used where Peso, Pegamentos, or the control summary require it.

## 5. Ferramentas

Ferramentas owns the canonical master records for CM, MF, BQ, PU, and CS as applicable. A Tool
record includes its stable identity, family/reference, lot, machine/line associations, technical
condition, and manually entered usage percentage.

Beta behavior:

- create, search, view, and edit Tool master data;
- create a new lot from an existing lot as a starting point;
- preserve Tool identity across downstream contexts;
- configure verification rules on a lot;
- keep production-specific fields in Job On rather than moving them into master data;
- keep manual usage as a manual value; never calculate or synchronize it implicitly.

Ferramentas is the master owner for BQ. Boquilhas does not become the BQ master merely because a
movement register is created.

## 6. Controlo

Controlo is one functional module. Its internal areas are Resumo/Folha de Controlo, Peso,
Pegamentos, Comparison, and Histórico. Comparison is a workflow inside Peso, not a separate module.

The Operador / Controlador prepares measurements, records technical OK/NOK, comments, and applicable
MCaliper links, then explicitly submits the sheet. The Responsável reviews, approves, rejects,
reopens, and decides. Technical OK/NOK is not an automatic production decision.

### 6.1 Peso

Peso operates within the Job On production context and records individual results per CM.

A `peso_id` persists through draft/edit/submit/approve/reject/reopen. It is not copied for approval.
A Peso is anchored to exactly one context at a time: the real production `cm_id`, or provisionally
the canonical `tool_id` while waiting for the matching Job On/CM context. Association to the real
`cm_id` is explicit; the pending Tool anchor is then cleared.

Inputs may include water weight, mold state, water temperature, nominal weight, established SAP or
previous-final reference values, notes, and technical reference values.

The functional calculations are:

```text
Capacity / Volume = water weight / temperature-table value
Glass weight = (CM capacity + Marisa/BQ volume - Punch/PU volume) * glass density
Cap volume = pi * sagitta^2 * (3 * radius - sagitta) / 3
```

Capacity/Volume and glass weight remain visible first-class values per CM. Temperature support is
5–35 °C. Water density is resolved automatically from the built-in temperature table; the operator
does not enter density or a divisor. Glass density comes from Controlo settings for the applicable
process and is frozen on the Peso at the first successful calculate/save. Later setting changes
affect new Pesos, not an existing Peso. The cap volume is informational and does not change the
principal glass-weight result.

Initial control happens before production. It records individual values, may show an informative
global average, and goes to the Responsável for a general decision. An average never hides an
individual result.

The Peso lifecycle uses `pendente`, `aprovado`, and `nao_aprovado`. Submit records who/when
without creating a new Peso; approve/reject are Responsável decisions; rejection requires a reason;
reopen returns the same Peso to `pendente`. Decision history is append-only. Once decided, the
original measurement rows, computed results, average, water temperature, frozen glass density,
volumes, submission facts, and decision remain historical facts and are never rewritten by
Comparação.

### 6.2 Comparison

Comparison is optional and happens during production as a new Peso workflow occurrence.

- Each occurrence has its own comparison record identity; one Peso may have multiple Comparação
  occurrences over time.
- It reuses the existing `cm_id` frozen in the same production context.
- It may cover one or several CMs.
- It uses the same Peso calculations.
- The Responsável decides each CM individually: Manter or Colocar de parte.
- Colocar de parte requires a justification.
- It never changes original Peso measurements, average, approval, PDF, or frozen facts.
- It never mutates Tool, Job On, or physical-stock state automatically.
- Comparison measurement rows are separate from the original Peso measurement rows and never enter
  the original Peso average.
- Every measured CM receives its own explicit final decision. A Comparação is only functionally
  complete when all measured subjects have a decision.
- There is no relation to a previous production Peso and no inferred pairing by CM number or row
  position.

### 6.3 Pegamentos

Pegamentos measures CM, BQ, and MF independently. The normal two-axis model is:

```text
Ovalização = Costura - Contra costura
Média = (Costura + Contra costura) / 2
```

Costura is 0° and Contra costura is 90°. If Contra costura is absent, the measurement remains valid:
Ovalização is absent/undefined and Média equals Costura. The tolerance corridor is nominal - 0.20
through nominal + 0.20; equality at a boundary is an alert. Alerts support human analysis and do
not automatically block production.

### 6.4 Resumo, decisions, and history

The summary covers exactly CM, BQ, MF, PU, and CS. PU/CS come from the exact Job On production
context, not from an independent control selection. Each piece can carry technical OK/NOK,
commentary, and an applicable MCaliper link.

The sheet states are:

```text
Rascunho -> Submetida -> Aprovada / Rejeitada
Submetida or decided -> Rascunho (reopen)
```

Submission is explicit. Rejection does not erase history. Reopening supports correction and a new
submission. The structured record/snapshot is authoritative; PDF is derived.

Histórico preserves the exact Job On context and revision used by the control. Corrections add facts
and do not silently rewrite earlier records.

## 7. Boquilhas

Boquilhas owns the manual external-repair movement register for BQ and its repairer settings. It
does not own the BQ master, production planning, or physical stock.

### 7.1 Pre-Job On association

Before a Job On exists, a register may be provisionally anchored to an existing canonical BQ
`tool_id`. When the matching BQ context is confirmed, the same register is associated to that
`bq_id`; the provisional Tool anchor is cleared, the version advances once, and movements already
recorded remain unchanged. No fake Job On or fake BQ context is created.

### 7.2 Closed movement vocabulary

Only these movements are valid:

1. **Saída** — BQ sent to the external repairer.
2. **Entrada** — repaired BQ returned.
3. **Entrada sem reparação** — BQ returned without repair.

There is no extra movement type, permanent standalone BQ lifecycle, irreparable Tool state, or
automatic quantity mutation. The register is quantity-based. Outstanding quantity is derived at
read time as:

```text
Saída - Entrada - Entrada sem reparação
```

It is never stored as a separate balance. A negative result is valid and visible; excess returns are
recorded completely and shown as a non-blocking discrepancy.

### 7.3 Settings and history

- Repairers are registered and renamed in Boquilhas; history is preserved.
- Machine assignments B1, B2, B3, C1, C2, and C3 are independent.
- Changing a default assignment does not rewrite historical movements.
- Operational actions are the same for Operador / Controlador and Responsável when Boquilhas is
  assigned.
- Movement history is append-only in functional effect; corrections add a new fact or an auditable
  cancellation rather than silently rewriting the past.

## 8. Admin

Admin is one assignable system module, not an operational production role. It contains:

- Users: create, edit, activate/deactivate, request password reset, and manage invitations;
- Templates: create/edit reusable access templates and associate one effective template per user;
- Applications: manage module presentation availability and order only;
- Audit: view filtered append-only business events and export a selected year when authorized.

The catalog never grants access by itself. Templates grant modules; profiles determine experience
inside granted modules. Passwords are never displayed or supplied by Admin. The first active Admin
cannot be removed or deactivated if that would leave no active Admin. Audit contains factual events,
not productivity scores or rankings.

## 9. Documents and Email

Structured records and snapshots are the source of truth. PDFs are derived outputs for presentation,
printing, and distribution. PDF generation must be repeatable and must not alter the structured
record.

The shared production document directory is:

```text
<base>/<reference>/<production-number>/
    Peso_<reference>_<machine>.pdf
    Resume_<reference>_<machine>.pdf
    Pegamentos_<reference>_<machine>.pdf
```

Only the base directory is configured manually. Reference and production subdirectories are
created/reused automatically. Job On and Controlo refer to the same production document
relationship; Job On does not own a duplicate document tree.

Directories are created by the server-side storage adapter. A file already at the target is not
silently overwritten. Sending is explicit and manual: the existing PDF is read, never regenerated
for sending, and routed automatically from machine B1/B2/B3 to group B or C1/C2/C3 to group C,
then to the configured template and recipient list. Unsupported or incomplete routing fails closed.

## 10. Non-Negotiable Boundaries

- Do not create parallel production or Tool identities.
- Do not create a new relation when an existing true relation can be continued; UUIDs connect
  context and persisted structures store genuinely new facts.
- Do not infer relations from visible labels, dates, latest records, or row positions.
- Do not turn internal Controlo areas into modules.
- Do not use dormant legacy Comparison code as a functional contract.
- Do not let UI hiding replace server authorization.
- Do not let warnings become blockers without an explicit functional rule.
- Do not make PDF or email transport the source of truth.
- Do not add excluded domains to Beta navigation, roadmap, migrations, or acceptance tests.
