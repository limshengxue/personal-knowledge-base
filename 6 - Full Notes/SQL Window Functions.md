2026-10-03 15:43

Tags: [[database]] [[sql]]

# SQL Window Functions
- Window functions calculate across related rows without collapsing each group into one output row.
- Use aggregates with `GROUP BY` to summarise groups; use windows to retain individual rows and add comparisons.
- `OVER` identifies a window calculation. [Window function tutorial](https://www.postgresql.org/docs/current/tutorial-window.html).

## Main Controls
- `PARTITION BY`: divide rows into independent calculation groups.
- Window `ORDER BY`: define the sequence within each group.
- Frame: specify the subset used by frame-sensitive calculations such as a running sum.
- Final query `ORDER BY`: control output presentation separately.

## Comparing the Same Weekday
Assume `daily_activity(activity_date, dollars)` has one row for every calendar day:

```sql
WITH compared AS (
  SELECT
    activity_date,
    dollars,
    LAG(dollars) OVER (
      PARTITION BY EXTRACT(ISODOW FROM activity_date)
      ORDER BY activity_date
    ) AS previous_week_dollars
  FROM daily_activity
)
SELECT
  activity_date,
  dollars,
  ROUND(
    100.0 * (dollars - previous_week_dollars)
    / NULLIF(previous_week_dollars, 0),
    2
  ) AS week_over_week_percent
FROM compared
ORDER BY activity_date;
```

- The percentage denominator is the previous value, not the current value.
- `LAG` returns the previous row in its partition, not necessarily seven days earlier if dates are missing.
- Use [[PostgreSQL Date Series and Gap Filling]] when a complete calendar is required.

## Common Pitfalls
- Windows operate after `WHERE`, grouping, and `HAVING`; use a subquery to filter their results.
- Keep earlier comparison rows in the inner query, then filter the displayed date range outside.
- For deterministic ranking, add a tie-breaker.
- For row-by-row running totals, specify `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` when that is the intended frame.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/1 - SQL|1 - SQL]]

