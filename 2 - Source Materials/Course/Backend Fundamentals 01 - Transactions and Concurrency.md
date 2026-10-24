2026-10-24 21:00

# Backend Fundamentals 01 - Transactions and Concurrency

## Headline
A transaction provides a commit boundary for a group of operations. It does not automatically make business logic correct or eliminate race conditions.

## Summary
This lesson covers race conditions, deadlocks, database transactions, ACID, and isolation levels. PostgreSQL-specific behavior is grounded in its official documentation. The central distinction is between atomicity, business invariants, and concurrency isolation: supporting transactions does not remove the need to design for concurrent requests. This source note preserves technical teaching material, not a private conversation transcript.

## Selling the Last Item
- With one item in stock, requests A and B can both read 1, each write the fixed value 0, and each create an order.
- At Read Committed, adding BEGIN/COMMIT alone does not eliminate this race condition.
- For a single-product allocation, use a conditional update that decrements stock only while stock > 0. Create an order only after confirming that exactly one row was updated.
- Keep the stock decrement and order creation in the same transaction so that an order failure also rolls back the decrement.

## Deadlocks and Lock Ordering
- A holds X while waiting for Y; B holds Y while waiting for X. This creates a cycle of waiting.
- PostgreSQL detects the deadlock and aborts one transaction. The application must handle the failure and, where safe, retry the entire transaction.
- Consistent lock ordering and short transactions reduce risk. Ordinary lock waiting is not itself a deadlock.

## The Responsibility Boundary of ACID
- A: All database changes in the transaction take effect together or are rolled back together.
- C: The state before and after the transaction satisfies constraints and business invariants.
- I: Concurrent transactions have defined visibility and interference guarantees.
- D: Acknowledged commits survive recovery within the configured durability guarantees.
- Counterexample: debiting 20 but crediting 30 can be atomic while violating the rule that a transfer must conserve funds.
- The database enforces declared constraints; the application must express correct business logic. The database does not automatically understand every business rule.
- ACID consistency is not replica synchronization or eventual consistency, and durability does not replace backups.

## PostgreSQL Isolation and Retries
- Read Uncommitted behaves as Read Committed.
- Ordinary queries at Read Committed use statement-level committed snapshots, so two queries in one transaction can see different states.
- Repeatable Read uses a stable snapshot but can still allow cross-row write skew, such as both on-call doctors going off duty.
- Serializable requires successful transactions to have an outcome equivalent to some serial execution; it does not physically execute all transactions one at a time.
- Two purchases may both read 1, but they cannot both commit an outcome that would be impossible under a valid serial execution.
- Retry the entire transaction, including reads and decisions, rather than only the last SQL statement.
- A database rollback cannot undo an external payment or an email that has already been sent. Retries must account for idempotency and external side effects.

## Key Ideas
- [[Race Conditions]]: State can change between checking a condition and acting on it.
- [[Deadlocks]]: Locks can form a circular wait.
- [[PostgreSQL Transactions and Isolation Levels]]: Transaction boundaries, ACID, isolation strength, and whole-transaction retries.
- Complete execution is not necessarily correct execution; a stable view does not guarantee that concurrent decisions are jointly valid.

## Remarks
- This is a source record of technical teaching material. Reusable understanding is integrated into the linked Full Notes.
- Official documentation provides verifiable references. Private chat links are not published in this public repository.

Co-authored by ChatGPT

# References
- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [PostgreSQL Deadlocks](https://www.postgresql.org/docs/current/explicit-locking.html#LOCKING-DEADLOCKS)
- [PostgreSQL Glossary](https://www.postgresql.org/docs/current/glossary.html)
