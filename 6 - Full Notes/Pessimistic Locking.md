2026-10-25 22:12

Tags: [[3 - Tags/database|database]] [[3 - Tags/concurrency|concurrency]]

# Pessimistic Locking

## Core Idea
Acquire a lock before reading and changing protected state so competing operations cannot modify that state during the critical transaction.

## Last-Item Example
Assume PostgreSQL Read Committed and a product with stock 1:

1. A begins a transaction and reads the product with SELECT FOR UPDATE.
2. B requests a conflicting lock on the same row and waits.
3. A checks stock, decrements it, creates an order, and commits.
4. B obtains the lock and reads the updated stock of 0, so it does not create an order.

```sql
BEGIN;
SELECT stock FROM products WHERE id = 42 FOR UPDATE;
-- Check stock while this transaction holds the lock.
-- If sufficient, decrement stock and insert the order here.
-- Otherwise, end without creating an order.
COMMIT;
```

This is a transaction outline, not a complete purchase implementation. All protected steps must use the same transaction and connection. An independently auto-committed locking SELECT does not protect subsequent work.

## What Is Blocked?
A PostgreSQL row lock blocks conflicting row-lock requests and relevant writes such as updates or deletes by other transactions. It does not by itself block an ordinary MVCC SELECT: readers can still see a committed version allowed by their snapshot.

The wait-and-see-updated-row example above is specific to Read Committed. Stronger isolation can instead require transaction retry after a concurrent change.

## Practical Rules
- Keep transactions short; do not hold locks while waiting for user input or unnecessary external calls.
- Acquire multiple locks in a consistent order to reduce [[Deadlocks]].
- Handle deadlock, timeout, and other transaction failures appropriately.
- Locking one row does not automatically protect all predicates or cross-row business rules.
- For a simple stock decrement, an atomic UPDATE conditioned on stock > 0 may avoid a separate locking read. Check the outcome and keep order creation in the same transaction.

## Compared with Optimistic Locking
[[Optimistic Locking]] detects a stale write at save time. Pessimistic locking coordinates access earlier, trading waiting and possible deadlocks for protection during the transaction. Neither term is a synonym for Serializable.

# References
[[2 - Source Materials/Course/Backend Fundamentals 02 - Locking and Database Performance|Backend Fundamentals 02 - Locking and Database Performance]]
- [PostgreSQL Explicit Locking](https://www.postgresql.org/docs/current/explicit-locking.html)
- [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
