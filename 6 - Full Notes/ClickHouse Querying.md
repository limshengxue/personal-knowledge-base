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
	- We should take advantage of the smart functions in ClickHouse (there are over 1500 custom functions)

### Format Output
- We can control the output format
```SQL
SELECT name, age FROM users FORMAT TabSeparated;
```

### CTE
- Common Table Expression can be used for identifier or result set
- ![[Attachments/Pasted image 20251122112705.png]]
### ClickHouse Functions
- There are 4 categories of ClickHouse function
- Regular - apply to each row separately like `lower`
- Aggregate - compute based on multiple rows like `quantile`
- Table - for creating table like `url`
- Window - window functions like standard SQL

We can check using `SELECT * system.functions`

#### Regular Functions
- apply to each row separately like `lower`
	- Arithmetic functions
	- Date and time functions
	- Array functions
	- String functions - fuzzy match, haystack search string

#### Aggregate Function
- Statistical function
- Exact vs approximation - for example `quantile` and `quantileExact`, `uniq`and `uniqExact` - the one without exact is approximation (faster)
- Count most frequent - `topK`
- Aggregate function combinators
	- Eg. *If* like`sumIf` which sum based on defined condition
- `any` functions used to include columns that are not in `group by` clause
- `arg` find not any value but specific condition like maximum `argMax`
- return the street that is most expensive in the town`SELECT town, argMax(street,price) FROM uk_price_paid GROUP BY town`

#### User defined functions
- `CREATE FUNCTION mergePostcode AS (p1, p2) -> concat(p1, p2)`


# References
[[2 - Basics Query]]
[[6 - Inserting Data]]
[[ClickHouse Functions]]
[[ClickHouse Queries]]
