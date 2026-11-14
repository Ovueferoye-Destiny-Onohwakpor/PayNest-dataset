# PayNest Dataset

Synthetic Nigerian B2C digital wallet/payments analytics portfolio project.

## Current status

- 17-table PostgreSQL architecture established.
- Schema is **NOT FROZEN**.
- Data Dictionary v1.0 is a working draft.
- Relational integrity specification is established/draft.
- PostgreSQL constraint-feasibility audit is complete.
- DDL has **not** started.
- Synthetic data has **not** been generated.
- Power BI is the planned analytics/visualization layer.
- Currency scope: NGN only.

## Current implementation gate

No PostgreSQL DDL or synthetic data generation should begin until:

1. The four state-transition matrices are formally approved.
2. The complete Data Dictionary is reconciled.
3. The integrity specification and ERD are validated against the final schema.
4. The schema receives explicit approval and is frozen.

## Repository structure

- `docs/current-state.md` — authoritative project-state checkpoint.
- `docs/next-steps.md` — active work queue.
- `docs/decisions/ADR-001-settlement-duplicate-evidence.md` — current settlement duplicate-handling decision.

## Source-of-truth warning

The previously generated visual ERD is **non-authoritative** because it contains known schema inaccuracies. The written reconciled schema/integrity specification takes precedence.

All project data will be synthetic. No production credentials, secrets, or personal data belong in this repository.
