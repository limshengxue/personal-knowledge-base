2026-10-27

Tags: [[3 - Tags/database|database]]

# Read Replicas

## Core Idea
Maintain another copy of data to serve suitable reads without making the primary execute every read query.

## Order-Service Example
1. The application writes a new order to the primary.
2. In PostgreSQL physical streaming replication, WAL changes travel to a standby and are replayed there.
3. A hot standby can answer read-only queries.
4. The application or routing layer explicitly sends eligible reads to that replica.

The replica is a copy, not a shard containing a different customer's records. [[Database Sharding]] distributes different subsets instead.

## Choose Reads by Their Requirements
A report that tolerates a slightly older view may be a good candidate. A just-completed checkout that must immediately show its own order needs a read-your-writes strategy. Authorization or inventory decisions must be designed for their correctness requirements rather than routed casually to a stale copy.

“Small list goes to primary, large list goes to replica” is not enough: freshness requirements, query cost, and actual load all matter.

Keep dependent write/read operations within the correct primary transaction. Separate connections do not magically share a transaction or snapshot.

## What Replicas Do Not Solve
- They do not distribute primary write capacity in the same way as sharding.
- [[Replication Lag]] means a replica may not yet show a committed write.
- Extra replicas do not automatically configure request routing or failover.
- They are not independent historical backups: mistaken changes can replicate too.
- Long-running standby queries and replay can conflict, so read capacity and replication behavior require observation.

See [[System of Record and Derived Data]] for the distinction between authority and copies.

# References
[[2 - Source Materials/Course/Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution|Backend Fundamentals 04 - Safe Migrations Replication and Data Distribution]]
- [PostgreSQL hot standby](https://www.postgresql.org/docs/current/hot-standby.html)
- [PostgreSQL streaming replication](https://www.postgresql.org/docs/current/warm-standby.html)
