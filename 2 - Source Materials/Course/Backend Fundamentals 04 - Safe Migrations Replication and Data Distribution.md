2026-10-27 21:00

# Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution

## Headline
Safe schema changes, replica reads, sharding, and partitioning address different operational problems; combining them requires explicit compatibility and consistency decisions.

## Summary
Lesson 4 covers concepts 16–20. A running service needs more than a valid final schema: old and new code may overlap, copied data may lag, and distributing storage changes how queries and transactions work. The follow-up develops a read-your-writes design and explains why a cached modification timestamp is not a reliable replica-progress marker.

This note preserves the technical teaching in English. Examples are illustrative and do not describe a verified deployment.

## 16. Zero-Downtime Migrations
- Example: replace phone with phone_number without breaking workers still using the old name.
- Expand with the new field, deploy compatible writes, cover every writer, and backfill existing records safely.
- Validate, switch reads, retain rollback compatibility, then remove the old field later.
- A backfill using stale captured values can overwrite a concurrent new value. Old-only writers can reintroduce divergence after a copy.
- “No downtime” does not mean “no locks.” Batch size, lock waits, latency, and database load are operational concerns.
- See [[Zero-Downtime Migrations]] for the staged example.

## 17. Read Replicas
- Replicas hold copies and serve suitable reads; they do not split the customer population into separate datasets.
- Primary writes are transported and applied to the copy.
- Request routing must be deliberately configured.
- A report may tolerate older data, while a just-submitted order confirmation may not.
- A live replica is not an independent historical backup. See [[Read Replicas]].

## 18. Replication Lag
- A successful primary commit can precede visibility on a replica.
- A missing new order immediately after checkout can therefore be a stale-read problem rather than failed storage.
- Route freshness-sensitive reads to the primary or verify the selected replica has applied the relevant write.
- Waiting a fixed number of milliseconds is not proof.
- Synchronous replication settings have different guarantees; durable receipt and query-visible application are not interchangeable.
- See [[Replication Lag]].

## 19. Database Sharding
- Split data across nodes, for example by tenant, to distribute storage and workload.
- Related tenant data should usually be placed together when common operations are tenant-local.
- Poor routing keys can create hotspots or fan-out queries.
- Cross-shard joins, global uniqueness, transactions, and resharding require additional design.
- A shard can have replicas and local partitions. See [[Database Sharding]].

## 20. Partitioning
- Split a logical table into physical pieces, such as monthly order partitions.
- A time-filtered query may skip unrelated pieces; an ID-only lookup may not benefit from that partition key.
- Partitioning does not remove the need for suitable indexes.
- Retention can work by partition, but detaching or dropping data has different consequences and is not a backup strategy.
- Native PostgreSQL declarative partitioning is not automatically multi-server sharding. See [[Table Partitioning]].

## Follow-up: Detecting Replica Freshness
The exercise described monthly order partitions plus a replica, followed by a new order missing on immediate refresh.

The proposed answer correctly identified replication lag, considered reading the primary, and suggested storing a last-edited timestamp in a cache to check whether the replica was safe to read.

Refinements:
1. Choose routing from freshness requirements and actual load, not only list size.
2. A cache can carry a freshness requirement; it cannot prove that requirement was met.
3. Business update timestamps need not follow transaction commit order. A replica can contain a later timestamp from another transaction while still missing the relevant write.
4. A reliable version check for one specific row establishes only that row's version, not completeness of an entire list.
5. For physical PostgreSQL replication, a post-commit WAL marker that covers the write can be compared with the selected replica's replay progress.
6. Read on that same replica with a fresh snapshot after it has caught up; use bounded waiting and primary fallback when appropriate.
7. Use “read replica” rather than “shard” for the copied database.

## Key Ideas
- [[Zero-Downtime Migrations]] is about compatibility over time.
- [[Read Replicas]] provide copies; [[Replication Lag]] governs when those copies show changes.
- [[Database Sharding]] distributes subsets across nodes.
- [[Table Partitioning]] organizes pieces of a logical table.
- Correctness requirements come before choosing a scaling mechanism.

## Remarks
- Course date: 2026-10-27, Malaysia time.
- The discussion demonstrates reasoning, not a claim of a deployed consistency protocol.
- A production WAL-marker design also needs routing, connection, and failover handling.

Co-authored by ChatGPT

# References
- [Expand-and-contract migrations](https://www.prisma.io/docs/guides/database/data-migration)
- [PostgreSQL ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html)
- [PostgreSQL hot standby](https://www.postgresql.org/docs/current/hot-standby.html)
- [PostgreSQL streaming replication](https://www.postgresql.org/docs/current/warm-standby.html)
- [PostgreSQL recovery information functions](https://www.postgresql.org/docs/current/functions-admin.html)
- [PostgreSQL WAL settings](https://www.postgresql.org/docs/current/runtime-config-wal.html)
- [Citus distribution keys](https://docs.citusdata.com/en/stable/sharding/data_modeling.html)
- [PostgreSQL table partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html)
