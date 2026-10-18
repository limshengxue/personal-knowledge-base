Tags: [[3 - Tags/database|database]] [[3 - Tags/indexing|indexing]]

# Append-Only Log Storage

## Core Idea
An append-only log writes each new record at the end of a file rather than overwriting older bytes. A minimal key/value store can append `(key, value)` updates this way, but finding the latest value by scanning the whole log gets slower as it grows.

## Why Append
Sequential writes are simple and can be efficient, especially compared with random writes on spinning disks. Immutable older records make it easier to distinguish complete writes from a partially written final record and to keep a history of changes. Deletions can be represented by tombstones rather than in-place removal.

Append-only storage needs a plan for locating records and reclaiming space. An index supports efficient lookup, while compaction removes superseded records from older segments. Crash recovery, concurrent writers, and atomicity of a record still require explicit handling.

## Related Concepts
- [[Hash Indexes and Log Compaction]]
- [[Event Streaming and Log-Based Storage]]
- [[Event Sourcing]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3 - Storage and Retrieval]]
