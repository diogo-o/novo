# Job On — Functional Rules

Functional authority for Job On. Access is in `shared/ACCESS_MODEL.md`; identities in
`shared/IDENTITY_AND_RELATIONS.md`; documents in `shared/DOCUMENTS_AND_EMAIL.md`.

## 1. Purpose and ownership

Job On is the production and planning hub. It owns the production occurrence, its revisions, selected
Tool contexts, production-specific configuration, checks, history, and production documents.

Job On does **not** own Tool master data, control results, or repair history. Later master edits must
not silently rewrite a historical production snapshot.

## 2. Planning and selection

- The calendar locates planned productions and their Job Ons.
- A click selects a day; an explicit action opens or creates the Job On. Changing month does not
  auto-select a day.
- Editing the production date updates the same Job On identity; it does not create a duplicate.
- A saved edit creates a new revision while preserving prior snapshots.
- Colors identify machine/line, not a hidden business state.

## 3. Production data

- The Job On stores the exact production/reference context, selected CM/MF/BQ plus lot, and
  production-specific fields such as FF, Calibres, Pinças, PU, CS, and TP/Tampão where applicable.
- PU, CS, and TP/Tampão are manually configured production fields. A duplicated Job On copies them as
  editable starting values; they are not permanent defaults.
- Job On keeps production-specific fields out of Tool master data.

## 4. Tooling and checks

- Tooling configuration selects CM/MF/BQ from options filtered by reference and machine/line.
  Filtering narrows options; it does not infer compatibility or choose a Tool.
- Configuring production data and tooling is a Job On configuration action. Confirming applicable
  checks is a separate action. Neither is a business role or job title; see
  `shared/ACCESS_MODEL.md`.
- A check rule belongs to the Tool lot; Job On materializes occurrences for this production.
- Check confirmation is manual and records the authenticated user and timestamp. It is never
  inferred from stock, repair, technical condition, usage, or elapsed time.
- Duplicating a Job On does not copy old check history; the new occurrence gets its own checks.
- Duplication creates a **new `jobon_id`**. The source contributes canonical `tool_id`
  selections, but the duplicate creates **new** CM/MF/BQ context identities from the **current** Tool
  master state rather than reusing or copying the source context identities.
- The source Job On and its frozen contexts remain unchanged.

## 5. Downstream context

Controlo consumes the exact Job On context and has no independent Job On selector. A control record
may inspect a different valid lot without changing the Job On plan or selected production tooling. The
same frozen context is used where Peso, Pegamentos, or the control summary require it.

## 6. Boundaries

- No Tool master edits from Job On.
- No second document tree; Job On consumes the shared production document relationship.
- No parallel production identity or `production_id`.
- Excluded domains never appear in Job On navigation or workflows.

## 7. Current implementation state

See `modules/job-on/IMPLEMENTATION_STATE.md`. Evidence does not create or override functional rules.
