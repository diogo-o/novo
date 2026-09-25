# Identity and Relations

This map defines the Beta identity graph. Visible fields help a person find and confirm a record;
stable relations are resolved and preserved by the server.

## Canonical Graph

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

## Ownership Table

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

## Safe Relation Rules

- Job On selects an existing Tool and freezes the resolved context.
- Downstream screens receive that context; they do not rebuild it from reference, lot, machine, or
  date.
- A duplicated Job On gets new occurrence/context identities but reuses the canonical Tool identity.
- A Comparison record reuses the existing `cm_id`; it does not create a prior-Peso relation.
- A provisional Boquilhas register uses a real BQ `tool_id`; association to a real `bq_id` preserves
  the same movement register and its facts.
- If a matching Tool or context does not exist, show a typed missing-context state. Never invent one.

## Forbidden Relations

- No identity made by concatenating family + reference + lot + machine.
- No independent Controlo production selector that competes with Job On.
- No Tool master edits from Job On or Boquilhas movement entry.
- No Comparison pairing by latest record, previous production, CM number, or table position.
- No document filename or email target treated as business identity.
