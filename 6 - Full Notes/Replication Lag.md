2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/concurrency|concurrency]]

# Replication Lag

## Core Idea
A write can be committed on the primary before a read replica has applied it.

## Example: The Missing New Order
The checkout writes successfully. An immediate list request reaches a replica that has not replayed the change, so the order appears absent. This is a visibility delay, not proof that the write failed or monthly partitioning lost the row.

## Read-Your-Writes Strategies
- Route freshness-sensitive reads to the primary.
- For PostgreSQL physical replication, obtain a WAL-position marker after successful commit that covers that commit. Before reading a selected replica, confirm its replay position has reached the marker.
- Then read on that same replica using a fresh snapshot. If it has not caught up, wait within a bound or fall back to the primary.
- A cache or session can carry the marker, but storing it does not prove replica progress.
- Coordinate connection routing and failover carefully; a marker protocol is not a complete failover design.

pg_last_wal_replay_lsn() reports applied progress. Receiving WAL is not the same as replaying it. An old transaction snapshot may still hide the change after replay.

## Why last edited time Can Mislead
Business timestamps need not follow commit order:
1. Transaction A sets updated_at to 10:00:01, then stalls before commit.
2. B sets 10:00:02 and commits first; the replica applies B.
3. A commits afterward but has not reached the replica.

The replica's maximum updated_at exceeds A's timestamp, yet A is missing. Comparing a cached timestamp with that maximum is not a reliable freshness proof.

Checking a reliable version on the exact row can prove that row reached the desired version, but does not establish completeness of a whole list or unrelated rows. Deletes also make a simple row-timestamp scheme inadequate.

## Other Pitfalls
- Sleeping one second is a heuristic, not a guarantee.
- Time since the last replayed transaction is not a perfect lag measure: an idle primary may have nothing new to send.
- With configured synchronous standbys, synchronous_commit = on waits for durable remote WAL, not necessarily query-visible replay. remote_apply waits for replay on the required synchronous standby set; it does not promise every replica or every pre-existing snapshot is fresh.
- More stringent waiting improves a specific guarantee but introduces latency and availability trade-offs.

See [[Read Replicas]] for routing and [[PostgreSQL Transactions and Isolation Levels]] for snapshots.

# References
[[2 - Source Materials/Course/Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution|Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution]]
- [PostgreSQL recovery information functions](https://www.postgresql.org/docs/current/functions-admin.html#FUNCTIONS-RECOVERY-INFO-TABLE)
- [PostgreSQL synchronous_commit](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT)
