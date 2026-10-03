2025-11-09 09:43

# 2 - Basics Query
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

# References
