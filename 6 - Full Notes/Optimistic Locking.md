2026-10-25 22:12

Tags: [[3 - Tags/database|database]] [[3 - Tags/concurrency|concurrency]]

# Optimistic Locking

## Core Idea
Let readers work concurrently, then accept a write only if the state it depends on has not changed. A version counter is a common way to detect stale updates without holding a lock while someone edits a form.

## Conditional Update Example
A and B both read stock 10 and version 7. A sells one item and successfully commits this update:

```sql
UPDATE products
SET stock = stock - 1,
    version = version + 1
WHERE id = 42
  AND version = 7
  AND stock > 0
RETURNING stock, version;
```

B's attempt using version 7 no longer matches after A commits. Returning no row signals that the condition was not satisfied, not that the sale succeeded. In this example, possible causes include a stale version, unavailable stock, or a missing product.

The comparison and modification belong in one statement. Checking the version first and later issuing an unconditional UPDATE reintroduces a [[Race Conditions|race condition]].

## Conflict Handling
- Reread current state and recompute the business decision before a bounded retry.
- For human edits, a conflict response and explicit merge may be safer than silently replaying a change.
- A successful UPDATE still requires transaction commit.
- Keep related changes, such as inventory allocation and order creation, in the same transaction.
- Every relevant writer must participate in version maintenance; an unrelated script that bypasses it can undermine the protection.

## Trade-offs and Limits
- Useful when conflicts are uncommon or editing spans user think time.
- Frequent conflicts can waste work through repeated retries.
- “Optimistic” does not mean writes are lock-free; the database can still lock internally.
- Version checking on one row does not enforce every multi-row invariant.
- SQLAlchemy's mapped version-counter feature applies to its individual-object flush path; do not assume all bulk update paths automatically use it.

## Related Concepts
- [[Pessimistic Locking]] acquires a lock before the protected operation.
- [[PostgreSQL Transactions and Isolation Levels]] explains transaction boundaries and stronger isolation.
- [[Race Conditions]] includes conditional stock decrement without an explicit version counter.

# References
[[2 - Source Materials/Course/Backend Fundamentals 02 - Locking and Database Performance|Backend Fundamentals 02 - Locking and Database Performance]]
- [SQLAlchemy Version Counters](https://docs.sqlalchemy.org/en/20/orm/versioning.html)
- [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
