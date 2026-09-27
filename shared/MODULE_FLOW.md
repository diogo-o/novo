# Module Flow

Functional authority for session entry, landing, navigation, and module composition.

## 1. Flow

```text
Authenticated session
        |
        +--> Admin-only grant --------> Admin
        |
        +--> Operational grant -------> Job On
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

## 2. Access path

1. The server resolves the active identity and the effective template grants (modules and
   permissions). See `shared/ACCESS_MODEL.md`.
2. The server chooses Job On or Admin as the landing surface.
3. The shell renders only granted top-level modules.
4. A direct request to an ungranted route returns access denied, not an empty screen.
5. Every mutating command repeats authorization, validation, and concurrency checks server-side.

## 3. Operational context

- Job On is the production hub and the source of the active production/revision context.
- Controlo receives that context when the user enters Controlo; entering Job On does not silently
  open Controlo.
- Controlo has no independent production reconstruction or selector.
- Ferramentas supplies master Tool options; Job On records the selected, frozen contexts.
- Boquilhas may show the relevant production context, but its movement entry does not mutate Job On.

## 4. Action capabilities by module

This replaces the previous profile-variant table. A capability is a module action granted through the
user's access template, not a business role. See `shared/ACCESS_MODEL.md`.

| Surface | Consultation / operational action | Configuration / decision action | Administration |
|---|---|---|---|
| Job On | Read the Job On and confirm applicable checks | Configure production data and tooling, select tools, edit, duplicate | No implicit access |
| Ferramentas | Consult where granted | Maintain master data and rules where granted | Govern access only |
| Controlo | Measure, record, submit (create action) | Review, approve, reject, reopen; decide each Comparison CM (approve action) | No implicit access |
| Boquilhas | Movement register — same operational actions for every granted user | Repairer/machine settings where granted | No operational actions |
| Admin | No | No | Users, Templates, Applications, Audit |

- Capabilities are not modules: Controlo's internal areas and workflows never become separate
  assignable modules.
- An Admin grant does not imply operational access, and an operational grant does not imply Admin.

## 5. Excluded from this flow

The Beta flow has no navigation, route, roadmap item, or acceptance dependency for the excluded
physical-stock, internal-repair, external-repair, plug (Tampões), or design-laboratory domains, and
no global História destination.

## 6. Open unknowns

- Whether every implemented surface is registered as available and reachable is not proven
  (`ModuleRegistrations.CurrentBuildAvailable`).
- Whether Ferramentas is registered as a top-level module or as contextual-only is an open
  discrepancy.
