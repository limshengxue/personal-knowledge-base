2025-06-30 20:28

# 8 - Indexing Strategy
- Indexing is thought of a data modelling activity
- It support consistency as constraints such as UNIQUE, PRIMARY KEY, EXCLUDE USING (allow no rows to have overlapping value) require backing index
- It also speed up retrieval by avoiding full table scan

## Indexing for Constraint
- PGSQL use MVCC (Multi-version Concurrency Control) to achieve isolation
- Each transaction view a snapshot of the database to prevent viewing inconsistent data produced by concurrent transactions while providing performance gain by avoiding locks
- When we declare constraint like UNIQUE, a backing index will be created
- The index is accessed using non-MVCC compliant way which allow consistency

## Indexing for Query
- Developer need to define this as PGSQL only create backing index by itself
- Each index introduce additional write cost, we need proper indexing strategy to avoid unnecessary cost
- We need to choose the proper query to optimize 
	- E.g. some analytical query can usually take longer time compared to user facing ones

### Creating Index
- An extension `pg_stat_statements` can help track the most commonly executed query and their execution time
- We can review the query plan using the `EXPLAIN` command
```
explain (analyze, verbose, buffers)
<query here>;
```

# References
