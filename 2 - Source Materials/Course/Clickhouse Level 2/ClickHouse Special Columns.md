2025-11-15 11:24

# ClickHouse Special Columns
## Default Columns
- Default value used when not provided

## Ephemeral Columns
- Mark a column as *ephemeral* and its value is not stored
	- The value will also not returned in SELECT
- Used as placeholder for incoming data that should be ignored
- Can be used with *materialized* columns

## Materialized Columns
- Calculated at *insert* time
- `Select *`query do not return materialized columns, unless we specify the column name
![[Attachments/Pasted image 20251115112759.png]]


# References
