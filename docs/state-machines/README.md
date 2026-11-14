# State-Transition Matrices

The four state machines are conceptually established but **not yet formally approved**.

Pending matrices:

1. Logical transaction — pending → successful / failed / cancelled
2. Processing attempt — pending → successful / failed / timed_out
3. Settlement — pending → confirmed / failed
4. Reversal — pending → completed / failed

Each matrix must define valid/invalid transitions, timestamp requirements, required operational events, late processor-response treatment, and reconciliation implications.

No DDL should encode these lifecycle rules until the matrices are formally approved.
