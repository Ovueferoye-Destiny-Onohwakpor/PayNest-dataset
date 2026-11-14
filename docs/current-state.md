## 4. Pending Tasks & Next Steps

### Immediate technical priority

- [ ] Formalize logical transaction state-transition matrix.
- [ ] Formalize processing-attempt state-transition matrix.
- [ ] Formalize settlement state-transition matrix.
- [ ] Formalize reversal state-transition matrix.
- [ ] Define valid and invalid transitions for every state.
- [ ] Define required timestamps for each transition.
- [ ] Define required operational events for each transition.
- [ ] Define late processor-response treatment in the formal state model.
- [ ] Define reconciliation treatment for multiple successful attempts.
- [ ] Define settlement status-change event requirements.
- [ ] Finalize settlement-source controlled values.
- [ ] Finalize settlement reference duplicate-detection semantics.
- [ ] Define idempotency-key generation mechanism/format.
- [ ] Define incident UUID generation policy.
- [ ] Finalize event-scope/context validation rules.
- [ ] Finalize transaction-type/rail mapping records.
- [ ] Finalize concurrency-safe attempt-number allocation.

### Schema completion

- [ ] Consolidate all 17 tables into the complete Data Dictionary v1.0.
- [ ] Ensure every table has every approved column.
- [ ] Specify PostgreSQL data type for every column.
- [ ] Specify nullability/defaults.
- [ ] Specify definitions and controlled values.
- [ ] Specify table grain.
- [ ] Specify PK/FK/UNIQUE/CHECK/index requirements.
- [ ] Audit normalization and redundant attributes.
- [ ] Validate all relationships.
- [ ] Validate all lifecycle rules against the transition matrices.
- [ ] Rebuild the visual ERD accurately after the schema is fully reconciled.
- [ ] Validate the visual ERD against the written integrity specification.
- [ ] Review and explicitly accept known design limitations.
- [ ] Obtain explicit schema approval.
- [ ] Freeze schema.

### Implementation gate

Only after explicit schema approval:

- [ ] Write PostgreSQL DDL.
- [ ] Implement PKs/FKs/composite FKs.
- [ ] Implement UNIQUE/CHECK/exclusion constraints.
- [ ] Implement required indexes.
- [ ] Implement triggers and controlled procedures.
- [ ] Test valid/invalid records.
- [ ] Build independent SQL data-quality checks.
- [ ] Generate synthetic data.
- [ ] Validate generated data.
- [ ] Develop analytical SQL.
- [ ] Build Power BI semantic model.
- [ ] Build Power BI dashboard.
- [ ] Perform analytical QA.

## 5. Active Code / State Objects

### Project state

```text id="w31zv4"
PROJECT: PayNest Portfolio Project

TABLE COUNT: 17

SCHEMA STATUS: NOT FROZEN

DATA DICTIONARY: Working Draft v1.0

RELATIONAL INTEGRITY SPECIFICATION: Draft / established

POSTGRESQL CONSTRAINT FEASIBILITY AUDIT: Completed

DDL STATUS: NOT STARTED

SYNTHETIC DATA: NOT GENERATED

RECONCILED ERD PDF:
PayNest_Final_Reconciled_ERD_v1.0.pdf

VISUAL ERD IMAGE:
Generated but NON-AUTHORITATIVE because it contains schema inaccuracies.

CURRENT NEXT DELIVERABLE:
Formalize transaction, attempt, settlement, and reversal state-transition matrices.

SCHEMA APPROVAL GATE:
Complete Data Dictionary
+
Integrity Specification
+
Validated ERD
+
Explicit schema approval

IMPLEMENTATION GATE:
No DDL or synthetic data before schema approval.
```

### Core table inventory

```sql id="ae5e7p"
CREATE TABLE dim_nigeria_geography (...);
CREATE TABLE dim_users (...);
CREATE TABLE user_kyc_history (...);

CREATE TABLE dim_channels (...);
CREATE TABLE dim_campaign_groups (...);
CREATE TABLE dim_campaigns (...);
CREATE TABLE user_acquisition_attribution (...);
CREATE TABLE fact_acquisition_spend (...);

CREATE TABLE dim_transaction_types (...);
CREATE TABLE dim_rails (...);
CREATE TABLE transaction_type_rails (...);

CREATE TABLE fact_transactions (...);
CREATE TABLE fact_transaction_attempts (...);

CREATE TABLE dim_event_types (...);
CREATE TABLE fact_transaction_events (...);

CREATE TABLE fact_transaction_settlements (...);
CREATE TABLE fact_reversals (...);
```

This is an inventory placeholder, not executable DDL.

### Core monetary constraints

```sql id="8sze0o"
-- Logical transaction
amount NUMERIC(18,2) NOT NULL CHECK (amount > 0),
fee_amount NUMERIC(18,2) NOT NULL CHECK (fee_amount >= 0)

-- Settlement
settlement_amount NUMERIC(18,2) NOT NULL
-- Cross-table:
-- settlement_amount = fact_transactions.amount

-- Reversal
reversal_amount NUMERIC(18,2) NOT NULL
-- Cross-table:
-- reversal_amount = fact_transactions.amount

fee_adjustment_amount NUMERIC(18,2) NOT NULL
-- Cross-table:
-- 0 <= fee_adjustment_amount <= fact_transactions.fee_amount
```

### Transaction completion consistency

```sql id="28glxt"
CHECK (
    (
        transaction_status = 'pending'
        AND transaction_completed_at IS NULL
    )
    OR
    (
        transaction_status IN (
            'successful',
            'failed',
            'cancelled'
        )
        AND transaction_completed_at IS NOT NULL
    )
)
```

### KYC temporal constraint

```sql id="yscs11"
CREATE EXTENSION IF NOT EXISTS btree_gist;

ALTER TABLE user_kyc_history
ADD CONSTRAINT ex_user_kyc_period
EXCLUDE USING GIST (
    user_id WITH =,
    tstzrange(
        effective_from,
        effective_to,
        '[)'
    ) WITH &&
);
```

### Required uniqueness / composite-FK support

```sql id="emzumh"
-- dim_campaigns
UNIQUE (campaign_id, channel_id)

-- fact_transactions
UNIQUE (tx_id, transaction_type_id)
UNIQUE (tx_id, user_id)

-- transaction_type_rails
PRIMARY KEY (transaction_type_id, rail_id)

-- fact_transaction_attempts
UNIQUE (attempt_id, tx_id)
UNIQUE (tx_id, attempt_number)
```

### Settlement duplicate handling

```text id="tg2eyk"
Do NOT use:

UNIQUE (settlement_source, settlement_reference)

Use an investigative index instead:

INDEX (settlement_source, settlement_reference)

Duplicate evidence must remain insertable so that reconciliation exceptions can be represented.
```

### Reversal uniqueness

```sql id="twe9qs"
UNIQUE (tx_id)
```

### Net fee revenue

```text id="2e0bwq"
Net Fee Revenue =
    Total Original Transaction Fees
    - Total Completed Reversal Fee Adjustments
```

### Current enforcement model

```text id="k13ygr"
DECLARATIVE:
    PK
    FK
    Composite FK
    UNIQUE
    CHECK
    Partial UNIQUE
    Exclusion constraints
    Indexes

CONTROLLED DATABASE LOGIC:
    Cross-table monetary validation
    Cross-table timestamp validation
    Lifecycle transitions
    Concurrency-sensitive attempt numbering
    Reversal eligibility
    Settlement/reversal reconciliation

INDEPENDENT QA:
    Geography completeness
    KYC temporal integrity
    Attribution integrity
    Attempt numbering
    State-transition audit
    Settlement/reversal reconciliation
    Referential consistency
    Fact-grain/fanout validation
```

### Critical design limitation

```text id="9yh2jk"
Duplicate settlements:
    Preserve all settlement evidence.
    Flag reconciliation exceptions.
    Do not automatically create multiple reversals.

Current reversal model:
    Maximum one full-principal reversal per transaction.

This is an explicitly accepted first-version scope limitation.
```

### Authoritative-source hierarchy

```text id="4pcnj1"
1. Current approved project decisions in session context
2. Current reconciled written schema/integrity specification
3. Latest validated Data Dictionary
4. Reconciled ERD PDF
5. Visual ERD image only after validation

The previously generated visual ERD is NOT authoritative where it conflicts with the written schema.