2026-10-03 15:43

Tags: [[database]] [[sql]]

# SQL Lateral Joins
- `LATERAL` lets a subquery in `FROM` refer to columns from a preceding table expression.
- It is useful for correlated top-N queries and table-returning functions.
- It describes a dependency, not a guarantee that the optimizer must use one particular physical join algorithm. [Lateral subqueries](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-LATERAL).

## Top Tracks per Genre
Assume the source's `genre`, `track`, and `playlisttrack` tables:

```sql
SELECT
  genre.name AS genre_name,
  popular.track_name,
  popular.playlist_count
FROM genre
LEFT JOIN LATERAL (
  SELECT
    track.trackid,
    track.name AS track_name,
    COUNT(playlisttrack.playlistid) AS playlist_count
  FROM track
  LEFT JOIN playlisttrack
    ON playlisttrack.trackid = track.trackid
  WHERE track.genreid = genre.genreid
  GROUP BY track.trackid, track.name
  ORDER BY playlist_count DESC, track.trackid
  LIMIT 5
) AS popular ON true
ORDER BY genre.name, popular.playlist_count DESC, popular.trackid;
```

## What Each Clause Means
- The inner `WHERE` uses the current genre.
- `LIMIT 5` chooses up to five tracks per genre; it does not mean tracks occurring more than five times.
- The tie-breaker makes selection reproducible.
- `ON true` adds no extra matching condition.
- `LEFT JOIN` retains genres with no matching tracks, returning null inner values.

## Alternatives and Cautions
- Use an ordinary join when there is no correlation.
- [[SQL Window Functions]] can also express top-N-per-group rankings.
- A later inner join to another table can accidentally remove null-preserved outer rows.
- Inspect the plan and relevant indexes for correlated queries; do not assume lateral is always faster.
- PostgreSQL table functions can reference preceding columns without an explicit `LATERAL` keyword, but writing it can clarify intent.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]
[[2 - Source Materials/Books/The Art of PostgreSQL/5 - A Small Application|5 - A Small Application]]

