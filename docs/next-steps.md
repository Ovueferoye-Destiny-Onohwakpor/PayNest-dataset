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
