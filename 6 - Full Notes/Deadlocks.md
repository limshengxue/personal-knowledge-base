2026-10-24 21:00

Tags: [[3 - Tags/concurrency|concurrency]] [[3 - Tags/database|database]]

# Deadlocks
A deadlock occurs when transactions wait for resources held by one another, forming a cycle that prevents them from making progress on their own.

### Example: Processing Two Products in Opposite Orders

1. Transaction A locks product X.
2. Transaction B locks product Y.
3. A tries to modify Y, but B holds its lock, so A waits.
4. B tries to modify X, but A holds its lock, so B also waits.

Ordinary UPDATE statements can acquire row locks. Explicit LOCK statements are not required for a deadlock to occur.

PostgreSQL detects deadlocks and aborts one transaction so that another can continue. Do not depend on a particular transaction always being selected as the victim.

### Common Responses
- Acquire locks in a consistent order, such as processing product IDs from smallest to largest, to avoid the opposite-order pattern above.
- Keep transactions short; do not hold locks while waiting for user input or a slow external API.
- Detect deadlock failures in the application. Where appropriate, use bounded retries that re-execute the entire transaction and its business decisions.
- Make retries safe for external side effects; do not accidentally charge a customer twice or repeat another irreversible action.

### Difference from Race Conditions
- **Race condition:** Interleaving produces an incorrect result.
- **Deadlock:** Circular waiting prevents progress.

Locking can solve some race conditions, but inconsistent lock ordering can introduce deadlocks. Waiting for one lock is not automatically a deadlock; the critical feature is a cycle of waiting.

## Related Concepts
- [[Race Conditions]]
- [[PostgreSQL Transactions and Isolation Levels]]
- [[Concurrency vs Parallelism]]

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
- [PostgreSQL Deadlocks](https://www.postgresql.org/docs/current/explicit-locking.html#LOCKING-DEADLOCKS)
