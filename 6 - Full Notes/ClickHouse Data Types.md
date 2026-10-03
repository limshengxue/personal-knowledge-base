2025-11-23 10:27

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Data Types
- Define a table engine [[ClickHouse Table Engine and Parts]] and data types of the columns to create a table and store data
- ClickHouse data types can be categorized into categories like Int, Decimal, String, etc
- ClickHouse is written in cpp, so it mimics the data types of cpp
- Choose a proper one based on the understanding of the data
- Use `Decimal` when exact decimal scale is required, such as monetary calculations. It is not automatically faster than `Float`; choose from correctness requirements and measured cost.

## Nullable
- Nullable sorting/primary-key columns are disabled by default for MergeTree. `allow_nullable_key` permits them when explicitly enabled, but consider their semantics and cost.
- If a value is missing for the nullable column, it will be NULL
- Under the hood, Nullable create an additional binary column to store whether the value is null or not 
	- Therefore, nullable come with a cost
- Nullable omit from calculation like average
- If we want to avoid nullable when can use `DEFAULT` but default will affect our calculation
- The default behaviour of Clickhouse is it will *insert default value of the data type*
![[Attachments/Pasted image 20251115111130.png]]

## Low Cardinality
- Useful when we have a column with a relatively small number of unique values
- Stores values as integers
	- Uses a dictionary encoding
- Advantage over Enums:
	- Can dynamically add new values
	- (No need to know all unique values at table creation time)
We can use the query below to check the space consumed by the table to know if `LowCardinality` columns helped save space
```SQL
SELECT
    table,
    formatReadableSize(sum(data_compressed_bytes)) AS compressed_size,
    formatReadableSize(sum(data_uncompressed_bytes)) AS uncompressed_size
FROM system.parts
WHERE table ilike 'uk_prices_%' AND active = 1
GROUP BY table
ORDER BY table;
```

## JSON
- JSON data type offers true column-oriented storage for JSON data
	- Performance and compression depend on data, queries, and the comparison configuration; no universal MongoDB/Elasticsearch ranking follows from the type.
![[Attachments/Pasted image 20251115111923.png]]

## Column Expressions
Defaults and insert-time computed values are covered in [[ClickHouse Default and Computed Columns]].

# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Data Types]]
[[ClickHouse Special Columns]]
[Decimal representation](https://clickhouse.com/docs/reference/data-types/decimal)
[Nullable key setting](https://clickhouse.com/docs/reference/engines/table-engines/mergetree-family/mergetree)
