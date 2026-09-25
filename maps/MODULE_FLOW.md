# Module Flow

```text
Authenticated session
        |
        +--> Admin-only user --------> Admin
        |
        +--> Operational user --------> Job On
                                      |
                                      +--> select production context
                                      |
                                      +--> Controlo
                                      |      +--> Resumo
                                      |      +--> Peso
                                      |      |      +--> initial control
                                      |      |      +--> optional Comparison workflow
                                      |      +--> Pegamentos
                                      |      +--> Histórico
                                      |
                                      +--> Ferramentas (master/context lookup)
                                      +--> Boquilhas (BQ repair movement register)
```

## Access Path

1. The server resolves active identity, profile, and effective template grants.
2. The server chooses Job On or Admin as the landing surface.
3. The shell renders only granted top-level modules.
4. A direct request to an ungranted route returns access denied, not an empty screen.
5. Every mutating command repeats authorization, validation, and concurrency checks server-side.

## Operational Context

- Job On is the production hub and the source of the active production/revision context.
- Controlo receives that context when the user enters Controlo; entering Job On does not silently
  open Controlo.
- Controlo has no independent production reconstruction or selector.
- Ferramentas supplies master Tool options; Job On records the selected, frozen contexts.
- Boquilhas may show the relevant production context, but its movement entry does not mutate Job On.

## Profile Variants

| Surface | Operador / Controlador | Responsável | Admin |
|---|---|---|---|
| Job On | Read and confirm checks | Configure, select, edit, duplicate | No implicit access |
| Ferramentas | Use where granted | Maintain master and rules | Govern access only |
| Controlo | Measure, record, submit | Review, approve/reject, decide | No implicit access |
| Boquilhas | Same operational actions as Responsável | Same operational actions as Operador | No operational actions |
| Admin | No | No | Users, Templates, Applications, Audit |

## Excluded From This Flow

The Beta flow has no navigation, route, roadmap item, or acceptance dependency for the excluded
physical-stock, repair, plug, or design-laboratory domains.
