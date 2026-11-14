# ADR-001: Preserve Settlement Duplicate Evidence

**Status:** Accepted for current v1 design

## Decision

Do **not** enforce `UNIQUE (settlement_source, settlement_reference)`.

Instead, retain all settlement evidence and provide a non-unique investigative index on `(settlement_source, settlement_reference)`.

## Rationale

The project explicitly needs to represent duplicate settlement evidence so that reconciliation exceptions can be detected rather than rejected at ingestion.

Example:

- Transaction principal: ₦10,000
- Settlement A: ₦10,000 confirmed
- Settlement B: ₦10,000 confirmed
- Confirmed settlement total: ₦20,000
- Result: reconciliation exception

A duplicate source/reference combination is therefore a QA/reconciliation signal, not automatically an invalid database record.

## Related limitation

The current v1 reversal model permits a maximum of one full-principal reversal per logical transaction. Duplicate settlement evidence must not automatically create multiple reversals.

## Scope

This decision supersedes any earlier draft/ERD that proposed a unique settlement source/reference pair.
