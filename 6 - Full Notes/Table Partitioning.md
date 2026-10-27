2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]]

# Table Partitioning

## Core Idea
Divide one logical table into manageable physical pieces while retaining a common table interface.

## Example: Orders by Month
An orders table is partitioned by created_at. October rows belong to one partition and November rows to another.

A query restricted to October can let PostgreSQL prune unrelated partitions. A query using only order_id may need to consider many partitions if it cannot restrict the partition key.

Partitioning and indexing solve different levels of selection: partitioning can eliminate whole pieces; suitable indexes can locate rows inside the remaining pieces.

## Operational Value
Age-based retention can operate at partition granularity rather than deleting millions of individual rows. Detaching a partition removes it from the parent's normal view without immediately destroying its standalone contents. Dropping it is destructive. Neither operation constitutes an independently recoverable backup.

Plan future partition creation and out-of-range inserts, and monitor maintenance and query planning overhead. Too many poorly chosen partitions can create costs rather than benefits.

## Boundaries
Native PostgreSQL declarative partitioning does not automatically create a distributed cluster. [[Database Sharding]] concerns placement across nodes; [[Read Replicas]] concern copies.

A suitable partitioning design depends on filtering and retention patterns, not only table size. Date partitioning is not a universal fix for every slow query or a missing fresh row; first investigate [[Replication Lag]] when writes succeed on the primary but immediate replica reads miss them.

## Related Concepts
- [[PostgreSQL Indexing Strategy]]
- [[Query Optimization]]
- [[ClickHouse Partition]] covers a different engine's partition behavior; do not transfer engine-specific assumptions blindly.

# References
[[2 - Source Materials/Course/Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution|Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution]]
- [PostgreSQL table partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html)
