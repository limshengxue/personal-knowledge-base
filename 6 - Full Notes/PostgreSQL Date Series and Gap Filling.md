2026-10-03 15:43

Tags: [[database]] [[sql]]

# PostgreSQL Date Series and Gap Filling
- Reports often need calendar dates even when no events occurred.
- Generate the expected dates first, then left join observations onto them.
- Decide whether a missing observation means zero or unknown before replacing it.

## Calendar-Driven Query
Assume `factbook` contains at most one row per date:

```sql
WITH calendar AS (
  SELECT generated_at::date AS report_date
  FROM generate_series(
    TIMESTAMP '2017-02-01',
    TIMESTAMP '2017-02-28',
    INTERVAL '1 day'
  ) AS dates(generated_at)
)
SELECT
  calendar.report_date,
  COALESCE(factbook.shares, 0) AS shares,
  COALESCE(factbook.trades, 0) AS trades
FROM calendar
LEFT JOIN factbook
  ON factbook.date = calendar.report_date
ORDER BY calendar.report_date;
```

- `generate_series` includes both boundaries when the step reaches them.
- Use `generate_series`, not the source note's prose name `make_series`.
- `COALESCE` selects the first non-null argument. Here it expresses the reporting policy that missing activity counts as zero. [Series functions](https://www.postgresql.org/docs/current/functions-srf.html).

## Avoid Accidental Data Loss
- If multiple observations exist per day, aggregate them before joining or the calendar row will be duplicated.
- Put filters on optional observations inside the join or a pre-aggregation query; a `WHERE` condition rejecting null observations can remove the missing dates.
- For timestamp events, define the reporting time zone before deriving a date.
- Local calendar days and fixed elapsed 24-hour intervals are not interchangeable across daylight-saving transitions.

## Applications
Use a complete date series for daily dashboards, inactivity checks, and weekday comparisons. [[SQL Window Functions]] can compare rows after the time series has the intended grain and coverage.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/1 - SQL|1 - SQL]]

