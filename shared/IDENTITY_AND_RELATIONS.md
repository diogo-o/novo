# Identity and Relations

Functional authority for the Beta identity graph and for the relations that may and may not be
created.

## 1. Canonical graph

```text
Tool master
  tool_id
    |
    +--> Job On occurrence
    |      jobon_id
    |        |
    |        +--> frozen CM context: cm_id
    |        +--> frozen MF context: mf_id
    |        +--> frozen BQ context: bq_id
    |
    +--> Ferramentas master maintenance

Job On context
  --> Controlo / Peso / Pegamentos / Resumo / Histórico
  --> production documents

BQ tool_id
  --> provisional Boquilhas movement register
  --> matching bq_id association when the Job On context exists
```

## 2. Identity rules

- `tool_id` is the stable, invisible identity of a canonical Tool master record. It is never
  generated from a concatenation or hash of type + reference + lot + machine.
- `jobon_id` is the stable identity of one Job On production occurrence. Editing the production
  date updates the same identity; it does not create a duplicate.
- `cm_id`, `mf_id`, and `bq_id` are frozen Tool contexts selected for that Job On
  occurrence/revision.
- Every new Job On, including duplication, creates **new** `cm_id`, `mf_id`, and `bq_id`
  snapshots from the **current** canonical Tool rows. The canonical `tool_id` may be reused; prior
  context identities are never reused and previous contexts remain frozen.
- Visible attributes (family, reference, lot, machine/line) are for search, filtering, and
  confirmation. They are not cross-module keys and are not a substitute for stable identity.
- CM/MF preserve both Tool/context identity and the individual piece number where the workflow needs
  a piece number. BQ repair movements are quantity-based.
- `peso_id` belongs to Controlo. In production it continues the existing `cm_id` relation; a
  pending pre-Job-On Peso may temporarily anchor to the canonical `tool_id` until a
  human-confirmed association to the real `cm_id` is possible.
- Each Comparison occurrence gets a new `comparacao_id` and reuses the existing `cm_id`.
- A Boquilhas register keeps the same register identity (`boquilhas_id`) when its provisional
  `tool_id` anchor is later replaced by the matching real `bq_id`. Do not confuse the register
  identity with the frozen Job On context `bq_id`.
- There is no `production_id`, no `previous_peso_id`, and no parallel identity used to recreate an
  already-existing relation.

## 3. Ownership table

| Identity or fact | Functional owner | Consumers | Rule |
|---|---|---|---|
| `tool_id` | Ferramentas | Job On, Controlo, Boquilhas | Stable Tool identity; never derived from visible fields. |
| `jobon_id` | Job On | Controlo, documents, downstream context | One production occurrence; edits preserve identity and revisions preserve history. |
| `cm_id` | Job On context | Peso, Comparison, Controlo | Frozen CM context for the exact production/revision. |
| `mf_id` | Job On context | Controlo and applicable downstream views | Frozen MF context for the exact production/revision. |
| `bq_id` | Job On context | Controlo and Boquilhas association | Frozen BQ context; provisional Boquilhas association resolves to the matching context. |
| Tool visible attributes | Ferramentas | Search/filter/confirmation | Reference, lot, family, and machine/line are not cross-module keys. |
| Production-specific fields | Job On | Controlo and documents | PU, CS, TP/Tampão, Pinças, Calibres, and notes belong to the production snapshot. |
| Control results | Controlo | Summary, history, PDF | Control owns measurements and decisions; it does not rewrite Job On. |
| BQ repair movements | Boquilhas | Boquilhas history and documents where applicable | Quantity ledger with the closed three-movement vocabulary. |

## 4. Safe relation rules

- Job On selects an existing Tool and freezes the resolved context.
- Downstream screens receive that context; they do not rebuild it from reference, lot, machine, or
  date.
- A duplicated Job On gets new occurrence/context identities but reuses the canonical Tool identity.
- A Comparison record reuses the existing `cm_id`; it does not create a prior-Peso relation.
- A provisional Boquilhas register uses a real BQ `tool_id`; association to a real `bq_id`
  preserves the same movement register and its facts.
- If a matching Tool or context does not exist, show a typed missing-context state. Never invent
  one.

## 5. Forbidden relations

- No identity made by concatenating family + reference + lot + machine.
- No independent Controlo production selector that competes with Job On.
- No Tool master edits from Job On or Boquilhas movement entry.
- No Comparison pairing by latest record, previous production, CM number, or table position.
- No document filename or email target treated as business identity.

## 6. Boundaries

- This document defines relations only. Access is in `shared/ACCESS_MODEL.md`; how modules compose is
  in `shared/MODULE_FLOW.md`; documents derived from these records are in
  `shared/DOCUMENTS_AND_EMAIL.md`.
