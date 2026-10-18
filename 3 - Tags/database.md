# Database

## Workloads and Data Flow
- [[OLTP and OLAP]] distinguishes operational requests from analytical scans.
- [[System of Record and Derived Data]] identifies authoritative data and its copies.
- [[Data Warehouses]] and [[Data Lakes]] cover analytical destinations.
- [[Materialized Views]] explains precomputed read results.

## Data Models and Queries
- [[Relational vs Document Data Models]] and [[Graph Data Models]] compare ways to represent relationships.
- [[Normalization and Denormalization]] explains the cost of copying facts.
- [[Star and Snowflake Schemas]] models analytical facts and dimensions.
- [[Declarative Query Languages]] explains how query engines choose execution plans.

## Storage and State Changes
- [[Append-Only Log Storage]] and [[Hash Indexes and Log Compaction]] explain log-based key/value storage.
- [[Event Sourcing]] and [[CQRS]] explain history-based state and separate read models.

## Related Topics
- [[3 - Tags/sql|sql]]
- [[3 - Tags/olap|olap]]
- [[3 - Tags/indexing|indexing]]
- [[3 - Tags/schema|schema]]
- [[3 - Tags/clickhouse|clickhouse]]
- [[3 - Tags/redis|redis]]
