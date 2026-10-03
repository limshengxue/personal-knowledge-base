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
	- When > 1000 parts, ClickHouse service stop letting you insert (max partition per insert)
- Partition with low cardinality value

![[Attachments/Pasted image 20251115113935.png]]

## Partition affects Insert and Merge
- When no partition, each *insert* create a part
- When we define partition, each *insert* create multiple parts
- When no partition, merge only limit by size (can potential merge into only 1 part)
- When partition was defined, merge limit by partition also
[[ClickHouse Table Engine and Parts]]


## Partition for Data Management
- We can delete, move, replace a specific partition


# References
[[6 - Full Notes/ClickHouse Partition|ClickHouse Partition]]
