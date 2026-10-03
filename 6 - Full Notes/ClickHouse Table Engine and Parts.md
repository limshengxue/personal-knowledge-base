2025-11-23 10:16

Tags: [[3 - Tags/clickhouse|clickhouse]] [[olap]] [[database]]

# ClickHouse Table Engine and Parts
- ClickHouse logically group tables into database

## Table Engine
- Each table has an engine
- Determines how and where data is stored
- `MergeTree` most commonly used table engine, data stored in ClickHouse, no streaming needed, so it is fast
- External-source engines such as `S3` and `PostgreSQL` query data outside local MergeTree storage. Performance depends on the source, network, and query, not a universal engine ranking.

### Merge Tree
- Merge Tree is the main table engine for single-node ClickHouse
	- Provides column-oriented, highly scalable performance
- Other variant of merge tree
	- Aggregating Merge Tree - rollup
	- Replacing Merge Tree - upsert
	- Shared Merge Tree - Clickhouse Cloud
	- Replicated Merge Tree - on prem replication

### Insert data into a MergeTree table
- Inserts can create one or more parts depending on partitions, block formation, and buffering.
- For synchronous inserts, start with batches of at least 1,000 rows and preferably about 10,000–100,000; tune to the workload. `async_insert` can provide server-side buffering.
- If we do small insert, it create many parts and require a lot of merge that consume resources and cause slower ingestion
- Part
	- Immutable folder containing column files and other metadata files
- Overtime, the part merged (we cannot control when the merging occur)
- Inactive part (merged part) will be deleted

#### Column Files
- Part folder contain column files 
- Which are immutable file that store the data (in column-oriented form)


## Inspect Active Parts and Storage
For a MergeTree-family table, `system.parts` exposes current part metadata.

Count active parts on the queried server:

```sql
SELECT count() AS active_parts
FROM system.parts
WHERE database = currentDatabase()
  AND table = 'uk_prices_1'
  AND active = 1;
```

Compare data sizes for the same table:

```sql
SELECT
  formatReadableSize(sum(data_compressed_bytes)) AS compressed_data,
  formatReadableSize(sum(data_uncompressed_bytes)) AS uncompressed_data,
  formatReadableSize(sum(bytes_on_disk)) AS total_on_disk
FROM system.parts
WHERE database = currentDatabase()
  AND table = 'uk_prices_1'
  AND active = 1;
```

- Restrict both database and table so identically named tables are not mixed.
- `active = 1` excludes replaced parts waiting for removal.
- Compressed and uncompressed data bytes exclude auxiliary files; `bytes_on_disk` measures a different, broader quantity.
- These are server-local results, not automatically totals across every replica.
- Rising active-part counts can motivate a review of insert batching and merge activity, but interpret them alongside partitioning and workload. [system.parts reference](https://clickhouse.com/docs/reference/system-tables/parts).

# References
[[2 - Source Materials/Course/Clickhouse Level I/3 - ClickHouse Architecture|3 - ClickHouse Architecture]]
[[2 - Source Materials/Course/Clickhouse Level I/4 - Clickhouse Data Parts|4 - Clickhouse Data Parts]]
[[2 - Source Materials/Course/Clickhouse Level I/5 - Queries for Parts and Primary Key|5 - Queries for Parts and Primary Key]]
[Batching and part management](https://clickhouse.com/resources/engineering/clickhouse-optimize-table-final)
