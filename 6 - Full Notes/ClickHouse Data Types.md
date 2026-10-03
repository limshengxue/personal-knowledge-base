2025-11-23 10:27

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Data Types
- Define a table engine [[ClickHouse Table Engine and Parts]] and data types of the columns to create a table and store data
- ClickHouse data types can be categorized into categories like Int, Decimal, String, etc
- ClickHouse is written in cpp, so it mimics the data types of cpp
- Choose a proper one based on the understanding of the data
- Prefer `Decimal` over `Float` for better precision and efficiency

## Nullable
- Nullable columns cannot be a part of primary key
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
	- Fast performance and great compression (better than MongoDB and ElasticSearch)
![[Attachments/Pasted image 20251115111923.png]]

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
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Data Types]]
[[ClickHouse Special Columns]]
