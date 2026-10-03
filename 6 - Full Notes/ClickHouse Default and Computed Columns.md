2026-10-03 18:49

Tags: [[3 - Tags/clickhouse|clickhouse]] [[database]] [[olap]]

# ClickHouse Default and Computed Columns
- Column expressions determine how incoming values become stored or computed values.
- They are separate from the storage types described in [[6 - Full Notes/ClickHouse Data Types|ClickHouse Data Types]].

## DEFAULT
- Supplies an expression when a column is omitted from an insert.
- An explicit value can be supplied instead.
- Defaults affect analytical results differently from a missing/null value.

## MATERIALIZED
- Computes the column from an expression during insertion.
- The value is stored with the row.
- Ordinary `SELECT *` excludes it by default; explicitly select the column when required.
- Check the relevant settings before assuming this visibility rule is immutable.

## EPHEMERAL
- Accepts an input value that is not stored as a normal column.
- Can supply an intermediate input for another column expression.
- It is not an ordinary persisted column available in later queries.

![[Attachments/Pasted image 20251115112759.png]]

## Example
```sql
CREATE TABLE order_values
(
    order_id UInt64,
    quantity UInt32 DEFAULT 1,
    unit_price Decimal(12, 2),
    total_price Decimal(18, 2) MATERIALIZED quantity * unit_price
)
ENGINE = MergeTree
ORDER BY order_id;

INSERT INTO order_values (order_id, unit_price)
VALUES (1, 12.50);

SELECT order_id, quantity, unit_price, total_price
FROM order_values;
```

Changing an expression does not mean every existing row is automatically recalculated; review ALTER behavior before changing production schemas.

# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Special Columns]]
[Column expressions](https://clickhouse.com/docs/sql-reference/statements/create/table)
