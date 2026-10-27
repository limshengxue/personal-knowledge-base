2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]]

# Query Optimization

## Core Idea
Make a query do less unnecessary work while preserving the required result; optimize measured bottlenecks rather than the appearance of SQL.

## Example: A Customer's Recent Orders
An endpoint needs the latest twenty orders for one customer, but loads every order and filters/sorts in Python.
1. Establish a baseline: request latency, query count, returned rows, and time spent waiting versus executing.
2. Move the selective customer filter, required ordering, and limit into SQL. Select only needed columns.
3. Inspect [[EXPLAIN ANALYZE]] with representative parameters and data.
4. Consider an existing or candidate index matching customer lookup and ordering. Test it rather than assuming every index helps.
5. Compare correctness and performance, including write overhead and unusually large customers.

A JOIN may eliminate [[N+1 Queries]], but joining several one-to-many collections can multiply result rows. Fewer statements do not automatically mean less total work.

## Diagnostic Questions
- Is the request repeatedly asking for related records?
- Does one statement scan or sort much more data than it returns?
- Are row estimates misleading the planner?
- Is latency actually a lock wait or a [[Connection Pooling]] acquisition wait?
- Does a proposed rewrite change NULL handling, duplicate rows, pagination, or ordering?

## Boundaries
A sequential scan is not inherently bad. Small tables and broad selections may favor it. Optimization does not mean forcing one plan forever: distribution, table size, cache warmth, and concurrency change the trade-offs.

Use [[PostgreSQL Indexing Strategy]] for access paths, and [[Database Migrations]] to deploy schema changes deliberately.

# References
[[2 - Source Materials/Course/Backend Fundamentals 03 - Query Plans Constraints and Migrations|Backend Fundamentals 03 - Query Plans Constraints and Migrations]]
- [PostgreSQL performance tips](https://www.postgresql.org/docs/current/performance-tips.html)
