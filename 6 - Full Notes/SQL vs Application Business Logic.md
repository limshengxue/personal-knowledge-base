2026-10-03 15:43

Tags: [[database]] [[sql]]

# SQL vs Application Business Logic
- SQL already expresses business choices through selected columns, predicates, joins, and ordering.
- The useful question is where a particular rule belongs, not whether a database may contain any logic.
- Treat PostgreSQL as a concurrent data-processing service rather than only a passive storage layer.

## Album-Duration Example
Assume the source's `album` and `track` tables:

```sql
SELECT
  album.albumid,
  album.title,
  COALESCE(SUM(track.milliseconds), 0) AS duration_ms
FROM album
LEFT JOIN track
  ON track.albumid = album.albumid
WHERE album.artistid = 1
GROUP BY album.albumid, album.title
ORDER BY album.title, album.albumid;
```

This returns one row per album in one round trip. Fetching albums and then querying tracks once per album introduces an N+1 query pattern.

## Good Database Candidates
- Filtering, joining, aggregation, and ranking over stored data.
- Constraints that all writers must respect.
- Atomic updates and rules requiring a clear transaction boundary.
- Shared data-access operations that benefit from [[PostgreSQL SQL Functions]].

## Good Application Candidates
- External API calls and coordination across systems.
- User interaction and presentation.
- Domain policies that are clearer and easier to test in application code.
- Work that would consume scarce database resources without benefiting from data locality.

## Evaluate Both Correctness and Cost
- Multiple application queries can observe different committed states; choose [[PostgreSQL Transactions and Isolation Levels]] deliberately.
- Measure round trips, transferred rows, database load, maintenance cost, and clarity.
- A set-based query is not automatically faster, and application logic is not inherently incorrect.
- Keep domain behaviour explicit even when data access is delegated to SQL; this complements [[Anemic vs Rich Domain Models]].

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]
[[2 - Source Materials/Books/The Art of PostgreSQL/2 - Software Architecture|2 - Software Architecture]]

