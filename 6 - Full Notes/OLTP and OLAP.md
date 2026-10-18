Tags: [[3 - Tags/database|database]] [[3 - Tags/olap|olap]]

# OLTP and OLAP

## Core Idea
Online transaction processing (OLTP) and online analytical processing (OLAP) describe different workloads. OLTP reads or modifies a small number of records as part of application operations. OLAP scans and combines many records to answer analytical questions. A transaction is a logical unit of reads and writes; it need not involve money.

## Different Access Patterns
- OLTP commonly uses indexed point lookups, short requests, and frequent updates. Correctness and predictable response time matter to user actions.
- OLAP commonly reads large data ranges, aggregates results, and supports varied, often exploratory queries. Layouts can favor scans and selected columns over point updates.

Running heavy analytical queries on the operational database can compete with user traffic. A separate analytical system can use different schemas, access controls, and storage layouts while reading a copy of operational data. The copy then needs an update pipeline and an acceptable freshness target.

## Related Concepts
- [[Data Warehouses]]
- [[ClickHouse Introduction]]
- [[PostgreSQL Transactions and Isolation Levels]]
- [[System of Record and Derived Data]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1.2 - Trade Off In Data Systems Architecture]]
