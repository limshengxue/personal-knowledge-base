Tags: [[3 - Tags/database|database]] [[3 - Tags/indexing|indexing]]

# Hash Indexes and Log Compaction

## Core Idea
A hash index maps each key to the byte offset of its latest value in an append-only data file. It turns an exact-key lookup from a full scan into a direct file read, provided the index fits in memory.

## Managing Growing Logs
Divide the log into segments. When an old segment stops accepting writes, compaction can retain only its latest record for each key. Merging several segments further removes superseded values. New files can be built in the background while readers use the old ones, then switched into service when complete.

A tombstone records deletion so older values do not reappear during lookup or compaction. Its eventual removal requires knowing that no older segment can still supply the deleted key. Since the hash map is in memory, startup may rebuild it from segments or load a saved snapshot.

## Limits
The number of indexed keys is constrained by available memory. Hash order does not support efficient range scans. Compaction saves space and bounds lookup work, but consumes background I/O and must coordinate safely with reads and writes.

## Related Concepts
- [[Append-Only Log Storage]]
- [[PostgreSQL Indexing Strategy]]
- [[ClickHouse Table Engine and Parts]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3 - Storage and Retrieval]]
