2025-11-23 10:19

Tags: [[clickhouse]] [[olap]] [[database]] [[indexing]]

# ClickHouse Granule and Primary Key
- MergeTree stores sorted data and uses a sparse primary index to avoid reading irrelevant ranges.
- Unlike a relational primary-key constraint, this key does not enforce uniqueness.

## Sorting Key and Primary Key
- `ORDER BY` defines the sorting key inside data parts.
- Without an explicit `PRIMARY KEY`, the sorting key also supplies the primary index key.
- A separately defined primary key must be a prefix of the sorting key.
- Extra sorting-key columns can influence storage ordering without being stored in the primary index.

![[Attachments/Pasted image 20251109104429.png]]

## Granules and Sparse Index Marks
- An index mark identifies a granule boundary using the key at the beginning of that range.
- `index_granularity` commonly defaults to 8192 rows, but adaptive byte limits can produce smaller granules.
- It is not a guarantee that every granule contains exactly 8192 rows or has its own execution thread.
- Read scheduling distributes ranges through the query pipeline.

![[Attachments/Pasted image 20251109102829.png]]

## Choosing Keys
- Start from frequent filters, ranges, and ordering requirements.
- Key-column order affects usable index prefixes; cardinality alone is not a universal ordering rule.
- Measure read rows, memory use, compression, insert costs, and representative query plans.
- A longer key consumes more index memory.
- [[6 - Full Notes/ClickHouse Partition|ClickHouse Partition]] primarily serves data management and is not a substitute for a useful sorting key.

## Alternative Access Paths
- Projections and materialized views can supply different physical layouts.
- A materialized view's destination table engine and sorting key determine its storage ordering; SELECT ordering alone is not that definition.
- Data-skipping indexes summarize ranges and are not additional primary indexes.
- Maintaining duplicate tables or derived layouts adds storage and update costs.

# References
[[4 - Clickhouse Data Parts]]
[MergeTree keys and granularity](https://clickhouse.com/docs/reference/engines/table-engines/mergetree-family/mergetree)
