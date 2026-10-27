2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]] [[3 - Tags/ci cd|ci cd]]

# Zero-Downtime Migrations

## Core Idea
Change the database through compatible stages so old and new application versions can coexist while requests continue.

## Example: Replace phone With phone_number
A direct rename can break old workers still selecting phone. A staged plan avoids making the code deployment and schema switch one indivisible instant:
1. Expand: add the new nullable column while retaining the old column.
2. Deploy compatible code that still reads the authoritative old field and writes both values in the same transaction.
3. Identify every writer, including jobs, scripts, and integrations. Retire old-only writers or provide a database-side compatibility mechanism.
4. Backfill existing rows in bounded, retryable batches. Use current values and concurrency-safe conditions, not stale values captured earlier in application memory.
5. Validate coverage and equality. Switch reads to the new field; continue compatible writes during the rollback window.
6. Contract only after old readers/writers and the rollback need are gone: remove the obsolete field in a later deployment.

## Why Backfill Ordering Matters
Suppose a batch reads an old phone value, a request changes both columns, then the batch overwrites the new column with its stale copy. The migration “completed” but lost the newer value.

Likewise, copying once while old-only writers remain allows those writers to make the columns diverge again. Retiring them or synchronizing their changes is part of correctness, not an optional deployment detail.

## Operational Limits
Zero downtime is a service objective, not a promise of no locks. PostgreSQL DDL lock requirements vary by operation; review them rather than assuming a short SQL statement is harmless. Bound lock waits, test on representative volume, and observe request latency, errors, database load, and replication lag.

Large backfills can be operational jobs coordinated with [[Database Migrations]], rather than one enormous transaction. Record progress and make recovery deliberate. A rollback plan must cover application compatibility and data, not only reverse DDL.

# References
[[2 - Source Materials/Course/Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution|Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution]]
- [Expand-and-contract guide](https://www.prisma.io/docs/guides/database/data-migration)
- [PostgreSQL ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html)
