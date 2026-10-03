2025-11-23 10:31

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Join
- Hash joins build on one input, so its size matters. Since 24.12 the planner can automatically place the smaller table on the right for supported joins; inspect the actual plan rather than relying only on manual table order.

## Join Algorithms
- ClickHouse join algorithms help to ensure maximum utilization of resources
- Default are parallel hash and hash
![[Attachments/Pasted image 20251115125029.png]]

![[Attachments/Pasted image 20251115125113.png]]


## Hash Join
- A hash table is built in memory from the right hand table
	- Hash: builds a single hash table
	- parallel_hash: buckets the data and builds multiple hash table
	- grace_hash: similar to parallel hash but limit memory usage
- The data in the right-hand table is streamed (in parallel) into memory
- The data in the left-hand table is streamed and joined by doing lookups into hash table
- NOTE:
	- In-memory hash implementations are limited by available/configured memory. `grace_hash` can spill partitions to disk; the fit-in-RAM rule is not universal.
	- But the *latest* version of ClickHouse will auto adjust according to table size

## Sort Merge
- Full sorting merge: both tables are sorted by join key
- Partial: the right hand table is sorted before joining
- Sort occur in memory if possibly, else spill to disk
- Full sort merge can have similar performance with *hash* but hash use more memory

## Direct Joins
- Ideal situations of joining
- 3 options
	- Dictionary
	- Join table engine (right hand table is stored in memory)
	- EmbeddedRocksDB table engine for the right

### Dictionaries
- Mapping of key values
- Storage depends on dictionary layout; not every dictionary is wholly in memory.
- Refresh behavior depends on source, layout, and lifetime configuration.
- Benefits
	- Easy and efficient to use
	- Efficient alternative to joining 2 table
	- Useful for low latency lookup queries
![[Attachments/Pasted image 20251115130810.png]]
![[Attachments/Pasted image 20251115131219.png]]

### Join table engine
- Special table engine used for JOIN
- Entirely stored in RAM
- Similar to a Dictionary, except
	- Not tied to a source (do not auto update)
	- Can value different values for same key
![[Attachments/Pasted image 20251115131647.png]]
![[Attachments/Pasted image 20251115131636.png]]

## Choosing suitable Join strategy
![[Attachments/Pasted image 20251115131800.png]]


# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Join|ClickHouse Join]]
[Join planning and algorithms](https://clickhouse.com/docs/concepts/best-practices/minimize-optimize-joins)
