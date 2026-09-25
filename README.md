# BA DMO — Beta Context

This repository is the clean, Beta-only context pack for Sol. It contains functional intent,
current implementation evidence, delivery order, and the minimum maps needed to work safely.

## Authority

1. The newest owner-confirmed Beta decisions.
2. `BETA_FUNCTIONAL_MANUAL.md`.
3. `CURRENT_STATE.md` as evidence of what DMO-MODULAR currently proves.

Implementation evidence never creates a functional rule. If a rule is absent here, it is unknown;
do not infer it from code, screenshots, prototypes, old plans, reports, or another repository.

## Beta Scope

Included: Job On, Ferramentas, Controlo, Boquilhas, Admin, documents/PDF/email, and the shared
identity, access, navigation, audit, and design-system rules needed by those areas.

Excluded from Beta: Armazém, Reparação Interna, Reparação Externa, Tampões, and Design Laboratório.
Those names must not be added to Beta navigation, workflows, roadmap items, or implementation
assumptions. A TP/Tampão field in a Job On production configuration is not the excluded Tampões
module.

## Files

- `BETA_FUNCTIONAL_MANUAL.md` — the only Beta functional manual.
- `CURRENT_STATE.md` — verified, partial, legacy, and missing implementation evidence.
- `ROADMAP.md` — ordered delivery plan with exit criteria.
- `FRONTEND_RULES.md` — non-negotiable shell, navigation, access, and interaction rules.
- `maps/IDENTITY_RELATIONS.md` — canonical identity and ownership map.
- `maps/MODULE_FLOW.md` — user and module flow.
- `maps/DOCUMENT_FLOW.md` — structured record, PDF, storage, and email flow.

## Working Rules

- Keep the Beta manual functional and implementation-agnostic.
- Use only the allowed Novo source and DMO-MODULAR evidence when updating this pack.
- Preserve existing canonical identities: `tool_id`, `jobon_id`, `cm_id`, `mf_id`, and `bq_id`.
- Do not add parallel identities, inferred relations, or fake records to make a screen appear complete.
- Keep unknowns explicitly open rather than resolving them by guesswork.
