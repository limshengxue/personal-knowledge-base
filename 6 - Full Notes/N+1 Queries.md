2026-10-25 22:12

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]]

# N+1 Queries

## Core Idea
An application fetches a list once, then performs another query for each item. The repeated round trips can make a request slow even when each individual query is fast.

## Example: Orders and Buyers
- Fetch 20 orders: 1 query.
- In a loop, fetch the buyer for each order: up to 20 more queries.
- The straightforward pattern performs 21 queries rather than deliberately loading the required data together.

ORM relationship access can trigger these queries implicitly. Identity-map reuse or preloaded data can change the exact count, so inspect actual emitted SQL.

## Two Common Fixes
### Join the Related Data
Retrieve orders and buyer information together. This is often suitable for many-to-one relationships such as one buyer per order.

### Batch the Related Query
1. Fetch the orders.
2. Collect distinct buyer IDs.
3. Fetch those buyers in one query and associate them with the orders in application code.

This small example usually needs two queries. Larger ID sets may require chunked batches.

SQLAlchemy provides strategies such as joinedload and selectinload. Choose based on the relationship and query shape rather than assuming all attribute access is free.

## Why One Giant JOIN Is Not Always Better
Joining multiple one-to-many relationships can multiply output rows. An order with 4 items and 3 events can produce 12 joined combinations, repeating order data. This increases transfer and processing work and makes pagination more delicate.

Optimize total work and latency, not merely the count of SQL statements. A small number of planned batch queries may outperform a single expansive JOIN.

## Optimization Discussion
For an endpoint that fetches 50 orders and then queries products individually, despite existing indexes:

1. Inspect the actual query pattern and eliminate unnecessary per-item work with a suitable JOIN or batch load.
2. Inspect remaining expensive queries and their execution plans. Verify the existing indexes before adding any.
3. If requests still wait for connections, investigate checkout duration, leaks, locks, and database headroom before increasing pool capacity.

This is not an inflexible priority order for every incident. Measurements may expose a different immediate bottleneck.

An index can accelerate each of 51 queries without reducing them to 2. A connection pool reuses connections without removing the redundant queries. [[Connection Pooling]] should normally be present from the start; “tune the pool last” refers to capacity tuning.

## Related Concepts
- [[PostgreSQL Indexing Strategy]]: improve access paths for the queries that remain.
- [[Connection Pooling]]: reuse and limit connection resources.
- [[Response Time and Tail Latency]]: evaluate end-to-end impact rather than individual-query speed alone.

# References
[[2 - Source Materials/Course/Backend Fundamentals 02 - Locking and Database Performance|Backend Fundamentals 02 - Locking and Database Performance]]
- [SQLAlchemy Relationship Loading](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html)
