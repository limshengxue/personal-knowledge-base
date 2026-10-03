2025-11-23 10:15

Tags: [[database]] [[olap]] [[3 - Tags/clickhouse|clickhouse]]

# ClickHouse
- ClickHouse started as Clickstream data warehouse
- OLAP (Online Analytical Processing)
	- Designed for analytical workloads rather than general OLTP. Atomic insert guarantees and limited transaction support exist under documented conditions.
- Database management system
- Open source under Apache License

## Features
- SQL-based
- Column-oriented
- Fast ingestion
	- Millions of rows per second
- Fast query execution
	- Performance depends on workload, schema, hardware, and baseline; a universal 1000x comparison is not established.
- Used for analytical workloads

### Column-Oriented
- Optimized for analytical type query which usually use specific columns
- Can be access quickly without scanning irrelevant column

## Use Case
- Real-time analytics
- Observability
	- Monitor log, traces, events
	- Detect anomalies, fraud, network or infrastructure issues
	- ClickStack : Application -> Otel Collector (Open Telemetry) -> ClickHouse -> UI Layer (HyperDx / Grafana)
- Data warehousing
- ML and GenAI
	- Execute fast and efficient vector search

## Using Clickhouse Service
- Create a new Service by choosing provider and region
- Configure the IP access list

## Database Engine
The default database engine is Atomic, unless you are using ClickHouse Cloud in which case the default database engine is Replicated.


## Storage and Query Model
- [[ClickHouse Table Engine and Parts]] explains MergeTree storage and merges.
- [[ClickHouse Granule and Primary Key]] explains sorting and sparse indexes.
- [[ClickHouse Querying]] covers ingestion and query examples.

# References
[[1 - What is ClickHouse]]
[Transactional conditions](https://clickhouse.com/docs/concepts/features/operations/insert/transactions)
