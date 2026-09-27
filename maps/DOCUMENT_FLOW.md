# Document Flow

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

## Source of Truth

- The structured record or production snapshot is authoritative.
- PDF is derived and can be regenerated for presentation or printing.
- A PDF mismatch never overrides the structured record.
- Email transport is distribution only; it never changes a measurement, decision, or production
  context.

## Production Document Directory

The shared deterministic production directory is:

```text
<base>/<reference>/<production-number>/
    Peso_<reference>_<machine>.pdf
    Resume_<reference>_<machine>.pdf
    Pegamentos_<reference>_<machine>.pdf
```

Only the base directory is configured manually. Reference and production folders are created or
reused automatically. Job On consumes the same production/revision document relationship; it does
not own a second document tree.

## Peso PDF

The Peso document target within that directory is:

```text
Peso_<reference>_<machine>.pdf
```

Rules:

- the reference, production number, and machine come from the exact Job On traversal facts;
- path segments are validated fail-closed;
- the server creates missing directories;
- an existing final file is never silently overwritten;
- a missing PDF is distinct from an unreadable workspace or failed read.

## Configuration and Sending

- The base directory is configured and checked in Controlo Definições as a server-host path.
- Email lists and templates are configured there; recipient addresses are never hardcoded.
- The user explicitly chooses Send after the control decision and existing PDF are available.
- Machine routing is closed: B1/B2/B3 map to B; C1/C2/C3 map to C; any other machine fails
  closed.
- Routing resolves exactly one group template and its associated recipient list.
- The existing PDF is attached verbatim. Sending never recalculates or regenerates Peso.
- Missing configuration, ambiguous templates, missing recipients, missing PDF, or transport failure
  returns a typed refusal and leaves the structured record unchanged.

## Module Document Boundaries

- Job On documents represent the exact production snapshot and revision.
- Controlo documents represent structured Peso, Pegamentos, and summary results.
- Boquilhas movement history remains a structured ledger; any document is derived from it.
- No document introduces a new business relation or becomes a master record.
