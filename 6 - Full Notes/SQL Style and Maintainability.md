2026-10-03 15:43

Tags: [[database]] [[sql]]

# SQL Style and Maintainability
- SQL is application code: review it for correctness, clarity, and change safety.
- Formatting should expose the query's structure rather than conceal it in one long line.
- Follow one consistent project style; uppercase keywords are a choice, not a correctness requirement.

## Readable Structure
- Put major clauses on separate lines.
- Use meaningful aliases and result-column names.
- Qualify columns where their ownership could be ambiguous.
- Keep predicates near the join or filtering step they describe.
- Use common table expressions when naming intermediate results improves understanding, not merely to increase nesting.

## Prefer Explicit Meaning
```sql
SELECT
  album.title AS album_title,
  artist.name AS artist_name
FROM album
JOIN artist
  ON artist.artistid = album.artistid
ORDER BY album.title, album.albumid;
```

- `ORDER BY 1` depends on select-list position and becomes fragile when columns move.
- `NATURAL JOIN` derives join columns from shared names; schema changes can silently alter its meaning.
- Explicit `ON` or carefully chosen `USING` columns make the relationship visible. [Join semantics](https://www.postgresql.org/docs/current/queries-table-expressions.html).

## Review Questions
1. What is the expected output grain: one row per album, event, or date?
2. Can joins multiply rows unexpectedly?
3. What should nulls, missing rows, and empty groups mean?
4. Is ordering deterministic where users depend on it?
5. Are values bound safely through [[SQL Parameterization and Prepared Statements]]?

## Maintenance Practice
- Document unusual business intent rather than paraphrasing obvious syntax.
- Keep query and schema changes reviewable through versioned migrations.
- Exercise edge cases and inspect performance-sensitive plans.
- Prefer clarity over cleverness; see [[KISS Principle]] and [[PostgreSQL Indexing Strategy]].

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/7 - SQL is Code|7 - SQL is Code]]

