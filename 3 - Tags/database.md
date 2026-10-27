# Database

## Workloads and Data Flow
- [[OLTP and OLAP]] distinguishes operational requests from analytical scans.
- [[System of Record and Derived Data]] identifies authoritative data and its copies.
- [[Data Warehouses]] and [[Data Lakes]] cover analytical destinations.
- [[Materialized Views]] explains precomputed read results.

## Data Models and Queries
- [[Relational vs Document Data Models]] and [[Graph Data Models]] compare ways to represent relationships.
- [[Normalization and Denormalization]] explains the cost of copying facts.
- [[Star and Snowflake Schemas]] models analytical facts and dimensions.
- [[Declarative Query Languages]] explains how query engines choose execution plans.

## Storage and State Changes
- [[Append-Only Log Storage]] and [[Hash Indexes and Log Compaction]] explain log-based key/value storage.
- [[Event Sourcing]] and [[CQRS]] explain history-based state and separate read models.

## Transactions and Concurrent Correctness
- [[PostgreSQL Transactions and Isolation Levels]] distinguishes atomicity, business invariants, isolation, and durability.
- [[Race Conditions]] explains stale check-then-act decisions and conditional inventory updates.
- [[Deadlocks]] explains circular lock waits and whole-transaction retry.

- [[Optimistic Locking]] checks for stale versions before accepting updates.
- [[Pessimistic Locking]] coordinates competing changes with explicit locks.

## Request-Level Database Performance
- [[Connection Pooling]] reuses connections and budgets concurrent database access.
- [[N+1 Queries]] identifies repeated queries and compares JOINs with batch loading.
- [[PostgreSQL Indexing Strategy]] selects access paths from workload evidence.

## Query Plans and Schema Integrity
- [[Query Optimization]] reduces measured unnecessary work.
- [[EXPLAIN ANALYZE]] compares planned and observed execution.
- [[Foreign Key Constraints]] protects relationships.
- [[Unique Constraints]] prevents duplicates for the intended business key.
- [[Database Migrations]] versions and tests changes, including Alembic head reconciliation.
- [[Zero-Downtime Migrations]] coordinates compatible rolling changes and backfills.

## Copies and Data Distribution
- [[Read Replicas]] moves suitable reads to copies.
- [[Replication Lag]] explains stale reads and read-your-writes safeguards.
- [[Database Sharding]] distributes subsets across nodes.
- [[Table Partitioning]] organizes one logical table into manageable pieces.

## Related Topics
- [[3 - Tags/concurrency|concurrency]]
- [[3 - Tags/sql|sql]]
- [[3 - Tags/olap|olap]]
- [[3 - Tags/indexing|indexing]]
- [[3 - Tags/schema|schema]]
- [[3 - Tags/clickhouse|clickhouse]]
- [[3 - Tags/redis|redis]]
