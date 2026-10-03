2026-10-03 15:43

Tags: [[database]] [[sql]]

# PostgreSQL SQL Functions
- A SQL-language function packages a database operation behind a named, typed interface.
- It can return a scalar, a row, or a set of rows.
- Prefer a set-based query when it directly expresses the operation rather than replacing it with a procedural loop.

## Table-Returning Function
Assume `album(albumid, artistid, title)` and `track(albumid, milliseconds)`:

```sql
CREATE OR REPLACE FUNCTION albums_for_artist(selected_artist_id bigint)
RETURNS TABLE (
  album_id bigint,
  album_title text,
  duration_ms bigint
)
LANGUAGE sql
STABLE
AS $$
  SELECT
    album.albumid::bigint,
    album.title::text,
    COALESCE(SUM(track.milliseconds), 0)::bigint
  FROM album
  LEFT JOIN track ON track.albumid = album.albumid
  WHERE album.artistid = selected_artist_id
  GROUP BY album.albumid, album.title;
$$;

SELECT *
FROM albums_for_artist(1)
ORDER BY album_title, album_id;
```

- Named parameters should not conflict with column names; qualify columns deliberately.
- `RETURNS TABLE` documents the result shape.
- `STABLE` is appropriate here for a read-only lookup, not for every function.
- Order results at the caller when presentation order matters. [SQL functions](https://www.postgresql.org/docs/current/xfunc-sql.html).

## Functions Are Not Procedures
- Call functions in expressions or table expressions using `SELECT`.
- Procedures use `CREATE PROCEDURE` and `CALL`.
- SQL functions cannot issue transaction-control commands such as `COMMIT`.
- A function call does not automatically upgrade the transaction's isolation level.

## Design Checks
- Version function definitions through database migrations.
- Test empty results, nulls, and duplicate-producing joins.
- Avoid marking table-reading functions `IMMUTABLE`.
- Keep permissions intentional and avoid introducing dynamic SQL unnecessarily.

Use [[SQL vs Application Business Logic]] to decide whether a shared database API is worthwhile and [[SQL Lateral Joins]] when a function depends on another table's current row.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]

