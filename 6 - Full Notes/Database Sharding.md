2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/software architecture|software architecture]]

# Database Sharding

## Core Idea
Split different subsets of data across database nodes so storage and work need not remain concentrated on one node.

## Example: Tenant-Based SaaS
A service stores tenants' orders and order items.
1. Choose tenant_id as the routing/distribution key when it matches the workload.
2. Route a tenant's operations to its assigned shard.
3. Co-locate that tenant's related tables to keep common joins and transactions local.
4. Add and rebalance capacity through an explicit data-placement process.

A routing map or distributed database manages placement; hard-coding a modulo formula without a resharding plan is not a complete design.

## The Key Determines the Trade-offs
A good key distributes load and keeps related operations together. A single huge tenant may still create a hotspot. A query without the key may need scatter/gather across shards.

Cross-shard joins, global uniqueness, and atomic transactions need system-specific coordination or revised application boundaries. Do not assume a constraint on one shard automatically enforces a global business rule.

Operational work also grows: resharding, migrations, backups, failure recovery, and observability must cover the distributed system.

## Distinctions
- [[Read Replicas]] copy data to more places; shards hold different subsets.
- [[Table Partitioning]] divides one logical table into pieces; PostgreSQL's ordinary declarative partitioning does not itself distribute those pieces across independent database servers.
- A shard can have replicas and its own table partitions. These techniques can be combined.

Do not shard simply because a row count sounds large. First identify the actual capacity limit and whether simpler query/index/storage changes suffice.

# References
[[2 - Source Materials/Course/Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution|Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution]]
- [Citus distribution keys and co-location](https://docs.citusdata.com/en/stable/sharding/data_modeling.html)
