2025-11-23 10:30

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Partition
Partitioning data improves query performance
- *However*, its main purpose is for data management. For query performance focus on defining good *primary key* [[ClickHouse Granule and Primary Key]]
- They help
	- Limit the number of granules searches
	- For data management
	- Improve performance of mutations, moving data around, retention policies
- Partition must be careful
	- `max_partitions_per_insert_block` limits partitions in one inserted block. Active-part limits such as `parts_to_throw_insert` govern a different condition; neither is a universal 1,000-part rule.
- Partition with low cardinality value

![[Attachments/Pasted image 20251115113935.png]]

## Partition affects Insert and Merge
- When no partition, each *insert* create a part
- An insert spanning several partitions can create separate parts for those partitions; an insert touching one partition need not create several parts.
- When no partition, merge only limit by size (can potential merge into only 1 part)
- When partition was defined, merge limit by partition also
[[ClickHouse Table Engine and Parts]]


## Partition for Data Management
- We can delete, move, replace a specific partition


# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Partition|ClickHouse Partition]]
[Partitions per insert](https://clickhouse.com/docs/reference/settings/session-settings/max-partitions)
