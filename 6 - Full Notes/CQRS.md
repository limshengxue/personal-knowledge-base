Tags: [[3 - Tags/database|database]] [[3 - Tags/software architecture|software architecture]]

# CQRS

## Core Idea
Command query responsibility segregation (CQRS) uses different models for changing state and reading it. A write model can prioritize validation and consistency; read models can be shaped for the queries users actually make. CQRS does not require event sourcing, although an event log is one way to feed read models.

## How It Works
A command enters the write side, where business rules decide whether to accept it. Accepted changes update one or more query-oriented projections. Those projections may be tables, caches, or materialized views tailored to different readers.

If projections update asynchronously, reads can temporarily lag behind writes. The system needs a freshness expectation, a way to rebuild projections, and a way to detect failed updates. Multiple read models reduce query work but increase the number of representations that must be operated.

## When It Helps
CQRS is useful when write correctness and read performance need materially different representations. For a simple workload, separate models can add more complexity than value.

## Related Concepts
- [[Event Sourcing]]
- [[Materialized Views]]
- [[System of Record and Derived Data]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
