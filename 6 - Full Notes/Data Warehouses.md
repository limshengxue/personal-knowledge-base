Tags: [[3 - Tags/database|database]] [[3 - Tags/olap|olap]]

# Data Warehouses

## Core Idea
A data warehouse keeps data from operational systems in a form suited to analysis. It lets analytical queries use different schemas, storage layouts, and access controls without consuming the operational system's capacity.

## How Data Arrives
An ETL pipeline extracts data from source systems, transforms it into an analysis-friendly model, and loads it into the warehouse. The warehouse is a derived representation, so its usefulness depends on the quality, freshness, and provenance of that pipeline.

Combining data from several operational systems can overcome data silos, but shared definitions are still needed: the same business term may mean different things in different sources. Analytical models such as fact and dimension tables help make these definitions usable for queries.

## Trade-offs
Separating workloads protects operational response time and permits different query patterns. It also creates duplication, pipeline maintenance, and potential delay between a business event and its appearance in analytics.

## Related Concepts
- [[OLTP and OLAP]]
- [[Data Lakes]]
- [[Star and Snowflake Schemas]]
- [[System of Record and Derived Data]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1.2 - Trade Off In Data Systems Architecture]]
