2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]]

# EXPLAIN ANALYZE

## Core Idea
EXPLAIN shows the planned work; EXPLAIN ANALYZE actually runs the statement and reports what happened.

## Practical Reading
For a read-only orders query in a suitable test environment:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT id, created_at
FROM orders
WHERE customer_id = 42
ORDER BY created_at DESC, id DESC
LIMIT 20;
```

- Compare estimated and actual row counts where the plan begins to diverge.
- Costs are planner units, not milliseconds.
- Actual time is reported in milliseconds; when loops exceed one, node timing and row counts are per-execution averages. Consider the repetition.
- Buffer information helps distinguish cache activity and reads; a read is not necessarily a physical storage-device read because the OS may cache it.
- Parent timings include descendant work. Do not sum all node timings as independent costs.

## Illustrative Investigation
A lookup producing five rows is individually cheap, but a nested loop invoking it ten thousand times may dominate the query. Ask why the outer side produces so many rows and whether estimates or query structure are wrong before replacing the join algorithm.

## Safety and Interpretation
ANALYZE is execution, not a preview: INSERT, UPDATE, DELETE, triggers, and called functions can have effects. Prefer safe representative data. Transaction rollback can undo ordinary transactional writes but not every external or nontransactional side effect; execution still consumes resources and takes locks.

One run is evidence, not a benchmark. Include representative parameters and workload conditions. [[Query Optimization]] uses the findings; it is not a contest to eliminate every sequential scan.

# References
[[2 - Source Materials/Course/Backend Fundamentals 03 - Query Plans Constraints and Migrations|Backend Fundamentals 03 - Query Plans Constraints and Migrations]]
- [PostgreSQL Using EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html)
- [PostgreSQL EXPLAIN command](https://www.postgresql.org/docs/current/sql-explain.html)
