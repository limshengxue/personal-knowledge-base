2025-11-09 11:08

Tags:

# 6 - Inserting Data
## Options for Insert Data
- Upload
- ClickPipes
- `clickhouse-client`
- Messaging service
- Integration service
- Migrate from another database
- Client application

## Table Function
- Allow constructing table
- Table Engine use a table function behind the scene
	- Be aware table engine like `s3` can be proxy to other db but they don't store data in clickhouse


## SQL
Create a temp memory engine table to allow schema inference
```SQL
CREATE TABLE weather_temp
ENGINE = Memory
AS
    SELECT *
    FROM s3('https://datasets-documentation.s3.eu-west-3.amazonaws.com/noaa/noaa_enriched.parquet')
    LIMIT 100
    SETTINGS schema_inference_make_columns_nullable=0;

-- Step 4
SHOW CREATE TABLE weather_temp;
```

Use the schema to create the mergetree table and ingest the data
```SQL
-- Step 5
CREATE TABLE weather
(
    `station_id` LowCardinality(String),
    `date` Date32,
    `tempAvg` Int32,
    `tempMax` Int32,
    `tempMin` Int32,
    `precipitation` Int32,
    `snowfall` Int32,
    `snowDepth` Int32,
    `percentDailySun` Int8,
    `averageWindSpeed` Int32,
    `maxWindSpeed` Int32,
    `weatherType` UInt8,
    `location` Tuple(
        `1` Float64,
        `2` Float64),
    `elevation` Float32,
    `name` LowCardinality(String)
)
ENGINE = MergeTree
PRIMARY KEY date;

-- Step 6
INSERT INTO weather
    SELECT * 
    FROM s3('https://datasets-documentation.s3.eu-west-3.amazonaws.com/noaa/noaa_enriched.parquet')
    WHERE toYear(date) >= '1995';
```



# References