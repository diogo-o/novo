# Ferramentas — Functional Rules

Functional authority for Ferramentas. Access is in `shared/ACCESS_MODEL.md`; identities in
`shared/IDENTITY_AND_RELATIONS.md`.

## 1. Purpose and ownership

Ferramentas owns the canonical master records for CM, MF, BQ, PU, and CS as applicable. A Tool record
includes its stable identity, family/reference, lot, machine/line associations, technical condition,
and manually entered usage percentage.

Ferramentas is the master owner for BQ. Boquilhas does not become the BQ master merely because a
movement register is created.

## 2. Beta behavior

- create, search, view, and edit Tool master data;
- create a new lot from an existing lot as a starting point;
- preserve Tool identity across downstream contexts;
- configure verification rules on a lot;
- keep production-specific fields in Job On rather than moving them into master data;
- keep manual usage as a manual value; never calculate or synchronize it implicitly.

## 3. Actions

- Master maintenance (create/edit master data and rules) is a module action granted through the
  access template, not a business role; consultation may be granted independently.
- Corrections of operational records and deliberate master edits are different actions and must not
  be treated as the same permission. A correction preserves audit/history.

## 4. Boundaries

- Ferramentas does not own production planning, production configuration, control results, or repair
  history.
- Ferramentas does not create a parallel Tool identity from visible attributes.
- Excluded domains never appear in Ferramentas navigation or workflows.

## 5. Current implementation state

See `modules/ferramentas/IMPLEMENTATION_STATE.md`. Evidence does not create or override functional
rules.
