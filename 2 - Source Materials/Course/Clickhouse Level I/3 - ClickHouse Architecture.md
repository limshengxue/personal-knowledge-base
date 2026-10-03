2025-11-09 09:56

# 3 - ClickHouse Architecture
- ClickHouse logicall group tables into database

## Table Engine
- Each table has an engine
- Determines how and where data is stored
- `MergeTree` most commonly used table engine, data in ClickHouse, no streaming needed, so it is fast
- Some other table engine like `S3`, `PostgresSQL` they store the data in the external place and just use ClickHouse as the *query engine*

### Merge Tree
- Merge Tree is the main table engine for single-node ClickHouse
	- Provides column-oriented, highly scalable performance
- Other variant of merge tree
	- Aggregating Merge Tree - rollup
	- Replacing Merge Tree - upsert
	- Shared Merge Tree - Clickhouse Cloud
	- Replicated Merge Tree - on prem replication


#### Insert data into a MergeTree table
- Each insert create a part (folder)
- Therefore, for better performance, data should be insert in batch (with >= 1m rows) or use `async_insert` which provide buffering
- If we do small insert, it create many parts and require a lot of merge that consume resources and cause slower ingestion
- Part
	- Immutable folder containing column files and other metadata files
- Overtime, the part merged (we cannot control when the merging occur)
- Inactive part (merged part) will be deleted


## Column-Oriented
- Optimized for analytical type query which usually use specific columns
- Can be access quickly without scanning irrelevant column



# References
