2026-10-25 22:12

Tags: [[3 - Tags/database|database]] [[3 - Tags/concurrency|concurrency]]

# Connection Pooling

## Core Idea
Reuse a bounded set of database connections. Borrow one for database work, complete the transaction, and return it instead of repeatedly establishing a new connection for every request.

A pool reduces connection-setup overhead and controls concurrent access to database resources. It is not a query-result cache.

## Request Lifecycle
1. Borrow a connection when database work requires it.
2. Execute the transaction on that connection.
3. Commit or roll back, then return it.
4. If the pool has no available capacity, another borrower usually waits until a connection returns or acquisition times out.

A thousand active website users do not necessarily require a thousand database connections. Reading a page or typing a form does not need to occupy a connection continuously.

## Application Lifetime and Transaction Scope
- In a typical FastAPI application, reuse the engine/pool for the process lifetime instead of creating a new pool per request.
- Scope sessions and transactions to appropriate units of work and close them on success and error paths.
- A transaction must remain on the same checked-out connection; independent transactions must not be interleaved arbitrarily on one connection.
- Avoid holding a connection or transaction while waiting for user input or unnecessary slow external API calls.
- Pooling is usually part of initial application design. Later tuning concerns pool limits and timeouts, not whether pooling should exist at all.

## Budget Across Processes and Replicas
If four application processes each have a pool capped at ten connections, their combined potential usage is forty, not ten. Include overflow settings, service replicas, workers, other applications, and operational headroom in the database-wide budget.

“Per process” is a common deployment pattern, not a universal law: count the actual pool instances and their limits.

## Diagnose Before Expanding
Measure connection acquisition wait, checkout duration, transaction duration, active database work, and database capacity.

Long waits may be caused by:
- leaked connections or incomplete cleanup;
- slow queries or lock waits;
- transactions held during unrelated work;
- genuinely insufficient pool capacity for a database that has spare capacity.

Increasing the pool does not fix leaks or [[N+1 Queries]]. It may worsen contention. Improve unnecessary work and long holds, then load-test capacity adjustments.

## Related Concepts
- [[PostgreSQL Transactions and Isolation Levels]]: a pooled connection carries transaction state.
- [[N+1 Queries]]: repeated requests to the database prolong work even when each query is indexed.
- [[PostgreSQL Indexing Strategy]]: access paths can reduce individual query cost.
- [[Pessimistic Locking]]: time spent waiting on locks can keep connections checked out.

# References
[[2 - Source Materials/Course/Backend Fundamentals 02 - Locking and Database Performance|Backend Fundamentals 02 - Locking and Database Performance]]
- [SQLAlchemy Connection Pooling](https://docs.sqlalchemy.org/en/20/core/pooling.html)
