# Controlo — Functional Rules

Functional authority for Controlo and all of its internal areas. Access is in
`shared/ACCESS_MODEL.md`; identities in `shared/IDENTITY_AND_RELATIONS.md`; documents in
`shared/DOCUMENTS_AND_EMAIL.md`.

## 1. One module, internal areas

Controlo is **one functional module**. Its internal areas are Resumo/Folha de Controlo, Peso,
Pegamentos, Comparison, and Histórico. Comparison is a workflow inside Peso, not a separate module.

Internal areas and workflows are never converted into modules.

## 2. Actions

Two distinct module actions exist inside Controlo:

- **Create action** — prepare measurements, record technical OK/NOK, comments, and applicable
  MCaliper links, then explicitly submit the sheet.
- **Approve action** — review, approve, reject, reopen, and decide.

These are granted actions, not business roles. Technical OK/NOK is not an automatic production
decision.

## 3. Peso

Peso operates within the Job On production context and records individual results per CM.

- A `peso_id` persists through draft/edit/submit/approve/reject/reopen. It is not copied for
  approval.
- A Peso is anchored to exactly one context at a time: the real production `cm_id`, or provisionally
  the canonical `tool_id` while waiting for the matching Job On/CM context. Association to the real
  `cm_id` is explicit; the pending Tool anchor is then cleared.
- Inputs may include water weight, mold state, water temperature, nominal weight, established SAP or
  previous-final reference values, notes, and technical reference values.

The functional calculations are:

```text
Capacity / Volume = water weight / temperature-table value
Glass weight = (CM capacity + Marisa/BQ volume - Punch/PU volume) * glass density
Cap volume = pi * sagitta^2 * (3 * radius - sagitta) / 3
```

- Capacity/Volume and glass weight remain visible first-class values per CM.
- Temperature support is 5–35 °C. Water density is resolved automatically from the built-in
  temperature table; the operator does not enter density or a divisor.
- Glass density comes from Controlo settings for the applicable process and is frozen on the Peso at
  the first successful calculate/save. Later setting changes affect new Pesos, not an existing Peso.
- The cap volume is informational and does not change the principal glass-weight result.

### Initial control

Initial control happens before production. It records individual values, may show an informative
global average, and goes to the approve action for a general decision. An average never hides an
individual result.

### Lifecycle

The Peso lifecycle uses `pendente`, `aprovado`, and `nao_aprovado`. Submit records who/when without
creating a new Peso; approve/reject are approve-action decisions; rejection requires a reason; reopen
returns the same Peso to `pendente`. Decision history is append-only. Once decided, the original
measurement rows, computed results, average, water temperature, frozen glass density, volumes,
submission facts, and decision remain historical facts and are never rewritten by Comparison.

## 4. Comparison

Comparison is optional and happens during production as a new Peso workflow occurrence.

- Each occurrence has its own comparison record identity; one Peso may have multiple Comparison
  occurrences over time.
- It reuses the existing `cm_id` frozen in the same production context.
- It may cover one or several CMs.
- It uses the same Peso calculations.
- Each CM receives an individual decision: **Manter** or **Colocar de parte**.
- `Colocar de parte` requires a justification.
- It never changes the original Peso measurements, average, approval, PDF, or frozen facts.
- It never mutates Tool, Job On, or physical-stock state automatically.
- Comparison measurement rows are separate from the original Peso measurement rows and never enter
  the original Peso average.
- Every measured CM receives its own explicit final decision. A Comparison is only functionally
  complete when all measured subjects have a decision.
- There is no relation to a previous production Peso and no inferred pairing by CM number or row
  position.
- `comparacao_id` and the reuse of `cm_id` follow `shared/IDENTITY_AND_RELATIONS.md`.

## 5. Pegamentos

Pegamentos measures CM, BQ, and MF independently. The normal two-axis model is:

```text
Ovalização = Costura - Contra costura
Média = (Costura + Contra costura) / 2
```

- Costura is 0° and Contra costura is 90°.
- If Contra costura is absent, the measurement remains valid: Ovalização is absent/undefined and
  Média equals Costura.
- The tolerance corridor is nominal - 0.20 through nominal + 0.20; equality at a boundary is an
  alert.
- Alerts support human analysis and do not automatically block production.

## 6. Resumo, decisions, and history

- The summary covers exactly CM, BQ, MF, PU, and CS. PU/CS come from the exact Job On production
  context, not from an independent control selection.
- Each piece can carry technical OK/NOK, commentary, and an applicable MCaliper link.
- The sheet states are:

```text
Rascunho -> Submetida -> Aprovada / Rejeitada
Submetida or decided -> Rascunho (reopen)
```

- Submission is explicit. Rejection does not erase history. Reopening supports correction and a new
  submission.
- The structured record/snapshot is authoritative; PDF is derived.
- Histórico preserves the exact Job On context and revision used by the control. Corrections add
  facts and do not silently rewrite earlier records.

## 7. Boundaries

- Controlo has no independent production reconstruction or Job On selector.
- Controlo never rewrites Job On.
- Internal Controlo areas never become modules.
- Excluded domains (physical stock, internal/external repair, plugs, design laboratory) are not part
  of Controlo.

## 8. Open unknowns

- **Peso reading pairing.** Older material described pairing water readings by their position in the
  reading table, explicitly not by the CM number. The clean Beta functional manual does not restate
  this rule. It is **UNKNOWN** here: do not infer it, and do not confuse it with the Comparison
  CM association, which is explicit and validated.
- **Resumo/Folha persistence.** Whether the summary is satisfied by the current read projection or
  requires a persisted Folha lifecycle (states, OK/NOK, observations, MCaliper links) is open.
- **Pegamentos implementation.** No verified implementation evidence exists.

## 9. Current implementation state

See `modules/controlo/IMPLEMENTATION_STATE.md`. Evidence does not create or override functional
rules.
