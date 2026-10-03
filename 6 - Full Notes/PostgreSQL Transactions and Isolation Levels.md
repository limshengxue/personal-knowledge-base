2026-10-03 15:43

Tags: [[database]] [[sql]]

# PostgreSQL Transactions and Isolation Levels
- A transaction groups operations into one commit or rollback boundary.
- Atomicity means all-or-nothing; consistency relies on appropriate constraints and application rules.
- Isolation controls interaction with concurrent transactions.
- Durability concerns persistence of acknowledged commits under the configured guarantees, not simply having backups. [Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html).

## PostgreSQL Isolation
| Level | Main behaviour |
| --- | --- |
| Read uncommitted | Behaves as read committed |
| Read committed | Default; each statement sees a new committed snapshot |
| Repeatable read | Uses a stable snapshot from the first non-transaction-control statement |
| Serializable | Successful transactions behave as an equivalent serial execution |

Serializable does not physically run every transaction one at a time. Conflicting operations can cause serialization failures that the application must handle. [Isolation semantics](https://www.postgresql.org/docs/current/transaction-iso.html).

## Example
For a report that must keep a stable snapshot across multiple queries:

```sql
BEGIN ISOLATION LEVEL REPEATABLE READ READ ONLY;
SELECT albumid, title
FROM album
WHERE artistid = 1;

SELECT albumid, SUM(milliseconds) AS duration_ms
FROM track
GROUP BY albumid;
COMMIT;
```

## Correctness Decisions
- At read committed, two queries can observe different committed states even inside the same transaction.
- A single set-based query can avoid some cross-query inconsistencies, but it does not replace all concurrency controls.
- Snapshot stability alone does not enforce every cross-row business invariant.
- Keep transactions short and avoid waiting for user input while holding them open.
- Retry the entire transaction after a serialization failure, with bounded retries and safe handling of external side effects.

Use [[SQL vs Application Business Logic]] to choose the query boundary and [[psql Workflows]] to understand interactive transaction state.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/2 - Software Architecture|2 - Software Architecture]]
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]
[[2 - Source Materials/Books/The Art of PostgreSQL/6 - Psql|6 - Psql]]

