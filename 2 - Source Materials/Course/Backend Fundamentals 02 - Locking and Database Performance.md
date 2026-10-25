2026-10-25 21:00

# Backend Fundamentals 02 - Locking and Database Performance

## Headline
Prevent conflicting updates, eliminate unnecessary database work, and tune resource capacity from measurements rather than treating every slow request as a connection-pool problem.

## Summary
Lesson 2 covers concepts 6–10: optimistic locking, pessimistic locking, connection pooling, N+1 queries, and database indexes. It extends the previous lesson's distinction between transactions and concurrency guarantees. The follow-up exercise clarifies that a connection pool is usually part of the initial application architecture; increasing its size is a later, evidence-based tuning decision, not the same as introducing pooling.

This note preserves the technical teaching and follow-up reasoning in English, not a private conversation transcript. Examples are illustrative, not measured production results.

## 6. Optimistic Locking: Check Before Accepting a Save
- Allow concurrent readers to work without holding a database lock throughout their editing time.
- When saving, require that the stored version still matches the version originally read.
- Example: A and B both read stock 10 at version 7.
  1. A conditionally changes stock to 9 and version to 8, then commits.
  2. B's update still requires version 7, so it no longer matches.
  3. The application detects the failed condition and rereads before deciding what to do.
- Compare the version and modify the row in one conditional UPDATE. A separate version check followed by an unconditional write still has a race window.
- An affected-row count of zero means a condition failed. Depending on the predicate, this may mean a stale version, insufficient stock, or a missing record; do not label every zero-row result as one specific cause.
- Useful for long-lived editing forms: do not hold a database transaction open while a person thinks.
- It does not mean the database uses no locks during writes.
- All relevant writers must maintain the version protocol. Retry only after reconsidering current state; do not blindly overwrite with the original stale value.

## 7. Pessimistic Locking: Coordinate Before Changing State
- Acquire a lock before the protected read/decision/update sequence, and retain it within the same transaction.
- Last-item example under PostgreSQL Read Committed:
  1. A begins a transaction and reads the inventory row with SELECT FOR UPDATE.
  2. B tries the same conflicting row lock and waits.
  3. A checks stock, decrements it, inserts an order, and commits.
  4. B obtains the lock and sees the updated stock of zero, so it does not create an order.
- Keep the entire sequence on the same transaction and connection. Committing immediately after locking does not protect a later update.
- A row lock does not, by itself, block ordinary MVCC SELECT queries; those can read the committed version visible to their snapshot.
- Keep transactions short. Do not wait for user payment or a slow external service while unnecessarily holding locks.
- For multiple rows, consistent acquisition order reduces deadlock risk; failures still need handling.
- Neither optimistic nor pessimistic locking is another name for Serializable. A single-row strategy does not automatically enforce all cross-row invariants.
- Simple stock allocation can often use one atomic conditional decrement instead of a separate locking read. Check success before creating the order, and keep both database changes in one transaction.

## 8. Connection Pooling: Borrow, Use, Return
- Reuse a bounded set of database connections instead of establishing and authenticating a new one for every request.
- Request lifecycle:
  1. Borrow a connection when database work begins.
  2. Execute queries and finish the transaction.
  3. Return the connection for another request to use.
- If all available connections are checked out, another borrower normally waits until a connection is returned or its acquisition timeout expires.
- Many simultaneous website users do not require one permanently assigned database connection each.
- In a typical FastAPI service, reuse the engine/pool across the application's process lifetime while correctly scoping and closing sessions/transactions.
- A transaction must continue on the same checked-out connection. Error paths must roll back and release resources.
- A pool is not a result cache and cannot make incorrect updates safe or slow SQL inherently fast.
- Capacity example: four processes, each with its own maximum of ten connections, can collectively reach forty. Include overflow allowances, other service replicas, background jobs, and operational headroom.
- Increasing the pool can increase database contention. Inspect acquisition wait, checkout duration, leaks, long transactions, and database capacity first.

## 9. N+1 Queries: Repeated Round Trips Hidden in a Loop
- Fetch twenty orders once, then fetch each order's buyer separately: one list query plus twenty related queries gives twenty-one queries.
- Even indexed individual queries can accumulate latency through repeated network round trips.
- ORM attribute access may trigger SQL, so inspect actual statements rather than judging only the apparent simplicity of application code.
- Two common alternatives:
  - JOIN the related data when fetching the list.
  - Fetch the list, collect distinct related IDs, and load those records in one batch; this small example usually becomes two queries.
- SQLAlchemy provides relationship-loading strategies such as joinedload and selectinload; select them based on relationship shape and observed SQL.
- One huge JOIN is not always best: multiple one-to-many relationships can multiply rows and complicate pagination.
- Large batches may be chunked. The goal is lower total work and latency, not a universal requirement of exactly one query.

## 10. Database Indexes: Maintain an Extra Access Path
- An index is an additional lookup structure, like a book's directory, rather than a free flag attached to a column.
- With a million orders, an index on buyer_id may let the database locate a particular buyer's records without checking every order.
- Indexes cost storage and maintenance when data is inserted or relevant values change.
- The planner can legitimately choose a sequential scan for a small table or a query needing a large share of rows.
- Start from actual filtering, joining, and ordering patterns. Inspect existing indexes before adding new ones.
- An index can help each query run faster, but it does not automatically turn twenty-one queries into two.

## Follow-up Exercise: JOIN, Index, Then Pool?
- Prompt: an endpoint fetches fifty orders, then queries products individually. The queries already have indexes, but the endpoint remains slow.
- The proposed answer was to solve N+1 with a JOIN, then consider indexes, then the connection pool.
- Refined reasoning:
  1. Remove unnecessary repeated queries using an appropriate JOIN or batch-loading strategy.
  2. Inspect the remaining slow queries and their execution plans. Because indexes already exist, verify suitability rather than automatically creating more.
  3. If latency remains, determine whether requests actually wait for a connection and why connections stay checked out. Increase capacity only when measurements and database headroom justify it.
- This is a diagnostic approach for this example, not a mandatory order for every incident. An observed connection leak or lock wait may deserve immediate attention.
- Pooling is normally used from the start. “Tune pool size later” does not mean “avoid a pool until all queries are optimized.”
- General principle: first distinguish incorrect results, unnecessary repeated work, expensive individual operations, and resource waiting.

## Key Ideas
- [[Optimistic Locking]] checks whether an update is based on stale state.
- [[Pessimistic Locking]] coordinates conflicting work before the protected change.
- [[Connection Pooling]] manages reusable connection capacity.
- [[N+1 Queries]] identifies unnecessary repeated database round trips.
- [[PostgreSQL Indexing Strategy]] selects access paths from workload evidence.
- These tools complement one another; none is a substitute for all the others.

## Remarks
- PostgreSQL-specific concurrency behavior is distinguished from general design principles.
- The follow-up demonstrates reasoning about one example, not verified mastery or a production performance result.
- Private chat links and unrelated personal conversations are not published.

Co-authored by ChatGPT

# References
- [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [PostgreSQL Explicit Locking](https://www.postgresql.org/docs/current/explicit-locking.html)
- [SQLAlchemy Version Counters](https://docs.sqlalchemy.org/en/20/orm/versioning.html)
- [SQLAlchemy Connection Pooling](https://docs.sqlalchemy.org/en/20/core/pooling.html)
- [SQLAlchemy Relationship Loading](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html)
- [PostgreSQL Index Introduction](https://www.postgresql.org/docs/current/indexes-intro.html)
