2025-11-23 10:15

Tags: [[database]] [[olap]] [[3 - Tags/clickhouse|clickhouse]]

# ClickHouse
- ClickHouse started as Clickstream data warehouse
- OLAP (Online Analytical Processing)
	- Strictly for analysis, *not transaction*, no idea of transaction as oppose to OLTP
- Database management system
- Open source under Apache License

## Features
- SQL-based
- Column-oriented
- Fast ingestion
	- Millions of rows per second
- Fast query execution
	- Typically 1000x faster than OLTP
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


# References
[[1 - What is ClickHouse]]