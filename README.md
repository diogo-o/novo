# BA DMO — Clean Functional Reference

This repository is the **clean functional reference** for **Sol**.

It contains the current functional truth of the product and nothing else:

- `MANUAL.md` — the functional manual (modules, operator flow, calculations, field meaning, UI behaviour, document behaviour).
- `CURRENT_STATE.md` — a very short implementation-status summary (`IMPLEMENTED` / `PARTIAL` / `NOT IMPLEMENTED` / `CURRENT WORK`).

## What this repository is

- A single, current, functional source of truth.
- The functional manual is the **main functional source**.
- Owner-confirmed workflow decisions that are **newer than the manual** override the manual only where they explicitly apply.

## What this repository is not

- It is **not** an architecture document.
- It is **not** an implementation contract.
- It is **not** a plan, report, workstream history or reconciliation record.

## Authority

1. Owner-confirmed functional decisions (the newest confirmed workflow rules).
2. The functional manual in `MANUAL.md`.
3. The current implementation is **evidence of what exists**, never a functional rule. Implementation detail must not be promoted into functional truth.

## Rules

- This repository does **not** link to the old DMO/BA repositories.
- This repository does **not** instruct anyone to consult historical repositories, plans, reports, designs or workstream documents.
- Information that is **absent** from this repository is **unknown**. Unknown means unknown: it must not be recovered, reconstructed or inferred from old repositories.
- Genuine open owner questions are recorded as open. They are not resolved by guessing.
