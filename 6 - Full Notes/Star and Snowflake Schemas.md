Tags: [[3 - Tags/database|database]] [[3 - Tags/olap|olap]] [[3 - Tags/schema|schema]]

# Star and Snowflake Schemas

## Core Idea
A star schema records analytical events or measurements in a fact table and describes their context in dimension tables. Each fact row refers to dimensions such as customer, product, place, or time. This retains detail for many later questions while giving analysts recognizable entities to group by.

## Variants
- **Star:** dimensions are kept relatively direct, making queries easier to navigate.
- **Snowflake:** dimensions are further split into related tables. This reduces some duplication but adds joins and model complexity.
- **One big table:** dimension attributes are copied into a wide fact-like table. Queries may be simpler, while storage and update costs increase.

Historical analytical data is often updated less frequently than operational records, so denormalization can be a reasonable choice. The right shape still depends on how queries filter and aggregate, how dimensions change over time, and which engine stores the data.

## Related Concepts
- [[Data Warehouses]]
- [[OLTP and OLAP]]
- [[Normalization and Denormalization]]
- [[ClickHouse Introduction]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
