# BA DMO — Beta Context

This repository is the Beta context pack for DMO-MODULAR: the functional intent, the current
implementation evidence, the delivery order, and the maps needed to work safely.

It is a context/documentation repository. It contains no application code, it does not migrate code,
and it does not define software architecture. Implementation facts are recorded as evidence only.

## Start here

- `INDEX.md` — the map of this repository: where every piece of information lives, how each
  document is classified, and which items are still unknown.

## Scope

Beta includes:

- Job On;
- Ferramentas;
- Controlo, with Peso, Comparison, Pegamentos, Resumo/Folha de Controlo, and Histórico as internal
  areas/workflows;
- Boquilhas;
- Admin;
- structured documents, PDF generation/storage, and explicit PDF email sending;
- the shared identity, access, navigation, audit, and design-system rules those areas need.

Beta excludes the physical-stock, internal-repair, external-repair, plug (Tampões), and
design-laboratory domains. Those names must not be added to Beta navigation, workflows, roadmap
items, or implementation assumptions. A TP/Tampão field in a Job On production configuration is a
production field, not the excluded Tampões module. See `governance/LEGACY_CONTAMINATION.md`.

## Authority

1. The newest owner-confirmed Beta decisions.
2. The module functional documents: `modules/*/FUNCTIONAL.md` together with the shared functional
   documents `shared/ACCESS_MODEL.md`, `shared/IDENTITY_AND_RELATIONS.md`,
   `shared/DOCUMENTS_AND_EMAIL.md`, and `shared/MODULE_FLOW.md`. Together these are the Beta
   functional authority, organized by module.
3. `modules/*/IMPLEMENTATION_STATE.md` as evidence of what DMO-MODULAR currently proves.

Implementation evidence never creates a functional rule. If a rule is absent here, it is unknown; do
not infer it from code, screenshots, prototypes, old plans, reports, or another repository.

Nothing under `archive/` is authority. Archived material is kept for traceability only.

## Access canon

`Utilizador → Template de acesso → Módulos/permissões`

Do not interpret the application as being based on fixed business roles such as `Operador` or
`Responsável`. Those names belong to a previous model and must not be reintroduced as the current
architecture. See `shared/ACCESS_MODEL.md`.

## Working rules

- Keep functional documents functional and implementation-agnostic.
- Use only the allowed Novo source and DMO-MODULAR evidence when updating this pack.
- Preserve the canonical identities: `tool_id`, `jobon_id`, `cm_id`, `mf_id`, and `bq_id`.
- Do not add parallel identities, inferred relations, or fake records to make a screen appear
  complete.
- Keep unknowns explicitly open rather than resolving them by guesswork.
- Organize information by module. Do not organize it by date, history, document origin, author, or
  tool.
- Never let archived or external material become authority by being cited.
