2026-10-03 15:43

Tags: [[database]] [[sql]]

# PostgreSQL Indexing Strategy
- Indexing serves two different goals: enforcing constraints and accelerating selected queries.
- Each index also costs storage and maintenance during writes.
- Choose indexes from actual access patterns rather than indexing every column.

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

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/8 - Indexing Strategy|8 - Indexing Strategy]]

