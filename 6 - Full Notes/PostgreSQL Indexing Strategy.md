2026-10-03 15:43

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]] [[3 - Tags/indexing|indexing]]

# PostgreSQL Indexing Strategy
- Indexing serves two different goals: enforcing constraints and accelerating selected queries.
- Each index also costs storage and maintenance during writes.
- Choose indexes from actual access patterns rather than indexing every column.

## Lookup Structure Example
An index is an additional maintained access structure, not a free attribute on a column. If a table contains a million orders and a query needs one buyer's orders, an index on buyer_id may locate candidate records without examining every order.

```sql
CREATE INDEX orders_buyer_id_idx ON orders (buyer_id);
```

This is a candidate for a workload filtering by buyer_id, not a recommendation to add it without checking existing indexes and data distribution. Inserts and relevant updates incur index-maintenance work, and the structure uses storage.

## Constraint Support
- Primary-key and unique constraints create supporting indexes.
- Exclusion constraints also use indexes to enforce their rules.
- A foreign key does not automatically create an index on its referencing columns; assess whether joins and parent-row changes justify one.
- MVCC provides snapshots, but PostgreSQL still uses locks; it is not a lock-free correctness model. [Constraint behaviour](https://www.postgresql.org/docs/current/ddl-constraints.html).

## Workload-Driven Process
1. Identify frequent or latency-sensitive queries; `pg_stat_statements` can help when installed and configured.
2. Inspect filters, joins, ordering, and the proportion of rows returned.
3. Check existing indexes before adding another.
4. Measure with representative data and up-to-date statistics.
5. Compare read improvements against write and storage costs.

An index is useful only when the planner judges it cheaper than alternatives. A sequential scan can be correct for small tables or queries returning many rows. [Index introduction](https://www.postgresql.org/docs/current/indexes-intro.html).

## Example
Assume an album listing filters by artist and orders by title:

```sql
CREATE INDEX album_artist_title_idx
ON album (artistid, title, albumid);

EXPLAIN (ANALYZE, BUFFERS)
SELECT albumid, title
FROM album
WHERE artistid = 1
ORDER BY title, albumid;
```

- This is a candidate to test, not a universal recommendation.
- `EXPLAIN ANALYZE` executes the statement. Use a safe environment and consider side effects, especially for writes or functions. [EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html).

Revisit indexes as workload and data distribution change.

## Relationship to Request-Level Performance
- [[N+1 Queries]] is a repeated-work problem: indexing every individual query does not eliminate repeated round trips.
- First identify whether the request is slow because of redundant queries, expensive individual statements, lock waits, or waiting for a connection.
- When queries already have indexes, inspect their suitability and actual execution plans rather than automatically adding more.
- [[Connection Pooling]] reuses connections but does not replace query optimization. Increase its capacity only when measured acquisition waits and database headroom justify doing so.
- There is no universal requirement to tune JOINs, indexes, and pool size in the same order for every workload.

## Related Query Diagnostics
- [[Query Optimization]] separates repeated work, expensive statements, and waiting.
- [[EXPLAIN ANALYZE]] explains plan estimates, actual rows, loops, and execution safety.
- [[Foreign Key Constraints]] and [[Unique Constraints]] distinguish relationship integrity from duplicate prevention.
- [[Table Partitioning]] can prune whole pieces while indexes locate rows within the remaining pieces.

# References
[[2 - Source Materials/Course/Backend Fundamentals 02 - Locking and Database Performance|Backend Fundamentals 02 - Locking and Database Performance]]
- [PostgreSQL Index Introduction](https://www.postgresql.org/docs/current/indexes-intro.html)
[[2 - Source Materials/Books/The Art of PostgreSQL/8 - Indexing Strategy|8 - Indexing Strategy]]

