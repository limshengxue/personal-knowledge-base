2025-11-23 10:16

Tags: [[3 - Tags/clickhouse|clickhouse]] [[olap]] [[database]]

# ClickHouse Table Engine and Parts
- ClickHouse logically group tables into database

## Table Engine
- Each table has an engine
- Determines how and where data is stored
- `MergeTree` most commonly used table engine, data stored in ClickHouse, no streaming needed, so it is fast
- Some other table engine like `S3`, `PostgresSQL` they store the data in the external place and just use ClickHouse as the *query engine* (they are slower)

### Merge Tree
- Merge Tree is the main table engine for single-node ClickHouse
	- Provides column-oriented, highly scalable performance
- Other variant of merge tree
	- Aggregating Merge Tree - rollup
	- Replacing Merge Tree - upsert
	- Shared Merge Tree - Clickhouse Cloud
	- Replicated Merge Tree - on prem replication

### Insert data into a MergeTree table
- Each insert create a part (folder)
- Therefore, for better performance, data should be insert in batch (with >= 1m rows) or use `async_insert` which provide buffering
- If we do small insert, it create many parts and require a lot of merge that consume resources and cause slower ingestion
- Part
	- Immutable folder containing column files and other metadata files
- Overtime, the part merged (we cannot control when the merging occur)
- Inactive part (merged part) will be deleted

#### Column Files
- Part folder contain column files 
- Which are immutable file that store the data (in column-oriented form)


# References
[[3 - ClickHouse Architecture]]
[[4 - Clickhouse Data Parts]]