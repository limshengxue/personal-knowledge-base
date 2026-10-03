2025-11-23 10:32

Tags: [[clickhouse]] [[database]] [[olap]] [[sql]]

# ClickHouse Querying
## Basics Query
Query from S3 file
```SQL
SELECT * FROM
s3('https://learn-clickhouse.s3.us-east-2.amazonaws.com/uk_property_prices/uk_prices.csv.zst')
LIMIT 1000;
```

View inferred schema
```SQL
DESCRIBE s3('https://learn-clickhouse.s3.us-east-2.amazonaws.com/uk_property_prices/uk_prices.csv.zst')
```

Create a table
- `MergeTree` is the most commonly used table engine. It allow high data ingestion rate.
```SQL
CREATE TABLE uk_prices_1
(
    `id` Nullable(String),
    `price` Nullable(String),
    `date` DateTime,
    `postcode` Nullable(String),
    `type` Nullable(String),
    `is_new` Nullable(String),
    `duration` Nullable(String),
    `addr1` Nullable(String),
    `addr2` Nullable(String),
    `street` Nullable(String),
    `locality` Nullable(String),
    `town` Nullable(String),
    `district` Nullable(String),
    `county` Nullable(String),
    `column15` Nullable(String),
    `column16` Nullable(String)
)
ENGINE = MergeTree
PRIMARY KEY date;
```

Ingest Data
```SQL
INSERT INTO uk_prices_1
    SELECT * 
    FROM s3('https://learn-clickhouse.s3.us-east-2.amazonaws.com/uk_property_prices/uk_prices.csv.zst');


```

## Select Queries
- Most select syntax in SQL works
- But
	- We need to ask different questions that we do with an OLTP
	- Choose functions suited to the workload; the available catalog depends on the installed version.

### Format Output
- We can control the output format
```SQL
SELECT name, age FROM users FORMAT TabSeparated;
```

### CTE
- Common Table Expression can be used for identifier or result set
- ![[Attachments/Pasted image 20251122112705.png]]
## Function Reference
Use [[ClickHouse Function Categories]] for scalar, aggregate, table, window, and user-defined functions.

## Example Schema Boundary
- The S3 example illustrates a staging schema, not a recommended final price representation.
- A string price compares lexicographically; cast or validate it into an appropriate numeric type before numeric ranking or aggregation.
- Confirm inferred source columns and their order before using `INSERT ... SELECT *`.
- See [[6 - Full Notes/ClickHouse Data Types|ClickHouse Data Types]] and [[ClickHouse Table Engine and Parts]] when defining the destination.

# References
[[2 - Basics Query]]
[[6 - Inserting Data]]
[[ClickHouse Functions]]
[[ClickHouse Queries]]
