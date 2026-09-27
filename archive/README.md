# Archive — non-authority material

Everything here is historical or raw. None of it is authority. It is kept only for traceability.

## Contents

| Item | What it is | Class | How to use it |
|---|---|---|---|
| `archive/AUDIT_REPORT_CONTEXT_INTEGRITY.md` | Read-only audit of the pack as it existed at `7bc8ddd` | REFERENCE ONLY / SUPERSEDED | Traceability only; its findings are resolved or tracked by the current pack. |
| `archive/qwen/DMO_BETA_IMPLEMENTATION_STATE_RECONSTRUCTION_ROADMAP_RAW.md` | Raw external DMO-MODULAR state reconstruction and plan | REFERENCE ONLY | Background on how evidence was gathered. Never the roadmap. |

## Removed from the active pack

- `qwen/DMO_BETA_SECOND_PASS_EVIDENCE_DEEPENING_RAW.md` and
  `qwen/PEGAMENTOS_RECONCILIATION_FOCUSED_EVIDENCE_RAW.md` — **deleted**. Each contained only the
  Qwen tool-error line "The requested file reference is not currently visible. Use files.search or
  files.list to rediscover the file, then retry with a returned ref_id or file_id." They were not
  evidence, and their file names falsely implied that a reconciliation and a second evidence pass had
  been produced.
- `BETA_FUNCTIONAL_MANUAL.md`, `CURRENT_STATE.md`, `ROADMAP.md`, `FRONTEND_RULES.md`, and
  `maps/*.md` — **superseded**, content redistributed into `modules/`, `shared/`, and
  `delivery/`. The previous pack is preserved in git at commit `e56ef46`.

## Never use these as current

- `MANUAL.md` — full-scope manual deleted in `a465195`; still reachable through git history. It
  contains the excluded domains and the old profile definitions.
- `reports/BETA_MASTER_RECONCILIATION.md` and `architecture/RECORD_LIFECYCLES.md` — referenced by
  archived material but not present in this repository; unresolved and stale.
- Any raw plan's migration numbers, steps, or acceptance criteria.

See `governance/LEGACY_CONTAMINATION.md` for the full registry.
