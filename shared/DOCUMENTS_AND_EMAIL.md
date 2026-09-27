# Documents and Email

Functional authority for structured records, PDF generation and storage, and explicit PDF email
sending.

## 1. Flow

```text
Structured production/control record
        |
        +--> server-side read model / frozen snapshot
        |          |
        |          +--> PDF generation (derived)
        |          |       |
        |          |       +--> deterministic server-host storage
        |          |       +--> explicit preview/print
        |          |
        |          +--> manual email send (existing PDF only)
        |
        +--> audit/history facts
```

## 2. Source of truth

- The structured record or production snapshot is authoritative.
- PDF is derived and can be regenerated for presentation or printing.
- A PDF mismatch never overrides the structured record.
- Email transport is distribution only; it never changes a measurement, decision, or production
  context.
- No document introduces a new business relation or becomes a master record.

## 3. Production document directory

The shared deterministic production directory is:

```text
<base>/<reference>/<production-number>/
    Peso_<reference>_<machine>.pdf
    Resume_<reference>_<machine>.pdf
    Pegamentos_<reference>_<machine>.pdf
```

Only the base directory is configured manually. Reference and production folders are created or
reused automatically. Job On consumes the same production/revision document relationship; it does not
own a second document tree.

## 4. Peso PDF rules

- The reference, production number, and machine come from the exact Job On traversal facts.
- Path segments are validated fail-closed.
- The server creates missing directories.
- An existing final file is never silently overwritten.
- A missing PDF is distinct from an unreadable workspace or a failed read.

## 5. Configuration and sending

- The base directory is configured and checked in Controlo Definições as a server-host path.
- Email lists and templates are configured there; recipient addresses are never hardcoded.
- The user explicitly chooses Send after the control decision and the existing PDF are available.
- Machine routing is closed: B1/B2/B3 map to group B; C1/C2/C3 map to group C; any other machine
  fails closed.
- Routing resolves exactly one group template and its associated recipient list.
- The existing PDF is attached verbatim. Sending never recalculates or regenerates Peso.
- Missing configuration, ambiguous templates, missing recipients, missing PDF, or transport failure
  returns a typed refusal and leaves the structured record unchanged.
- No persistent send table is required; send evidence (file, template, recipients, group, timestamp)
  is returned by the send action.

## 6. Module document boundaries

- Job On documents represent the exact production snapshot and revision.
- Controlo documents represent structured Peso, Pegamentos, and summary results.
- Boquilhas movement history remains a structured ledger; any document is derived from it.
- No document introduces a new business relation or becomes a master record.

## 7. Current implementation evidence

- **IMPLEMENTED EVIDENCE.** Deterministic naming, server-host storage, directory creation, no silent
  overwrite, and explicit existing-file reads are implemented. Manual email sending reuses the
  existing PDF and resolves machine group B or C to its configured template and recipient list.
- **PARTIAL / NOT PROVEN.** PDF settings, lists, templates, and sending have source evidence.
  End-to-end acceptance across every Controlo document and every production field is not proven.

Evidence references (external DMO-MODULAR source, not present in this repository and not pinned to a
commit):

- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfNaming.cs`
- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfFileStore.cs`
- `DMO-MODULAR/src/DMO.Application/Documents/PesoPdfSendService.cs`
