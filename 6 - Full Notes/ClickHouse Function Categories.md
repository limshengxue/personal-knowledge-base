2026-10-03 18:49

Tags: [[3 - Tags/clickhouse|clickhouse]] [[database]] [[sql]]

# ClickHouse Function Categories
- Functions express transformations, summaries, and data-source access in [[ClickHouse Querying]].
- Choose a function according to the intended row grain and correctness requirement.

## Scalar Functions
- Operate on argument values, usually producing a result per row.
- Examples include arithmetic, date/time, string, and array functions.
- `lower(name)` normalizes text; `concat` combines strings.

## Aggregate Functions
- Summarize groups: `sum`, `quantile`, `uniq`, and `topK`.
- Approximate and exact variants have different accuracy/resource tradeoffs.
- Combinators such as `sumIf` add conditional behavior.
- `any` does not select a predictable representative row.
- `argMax(value, weight)` returns a value associated with a maximum weight; ties need deliberate handling.

For a table with a numeric `price` column:

```sql
SELECT town, argMax(street, price) AS highest_price_street
FROM uk_price_paid
GROUP BY town;
```

Do not rank string prices as if they were numeric.

## Table and Window Functions
- Table functions such as `s3` and `url` return a queryable table expression. They do not inherently create a persistent table.
- Window functions retain individual rows while comparing related rows; see [[SQL Window Functions]].

## Discover and Define
```sql
SELECT *
FROM system.functions;

CREATE FUNCTION mergePostcode AS (first_part, second_part) ->
    concat(first_part, second_part);
```

Check the installed function catalog and permissions; the available functions vary by release.

# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Functions]]
[Function overview](https://clickhouse.com/docs/sql-reference/functions/overview)
