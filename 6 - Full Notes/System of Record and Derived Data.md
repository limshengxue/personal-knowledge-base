Tags: [[3 - Tags/database|database]] [[3 - Tags/data streaming|data streaming]]

# System of Record and Derived Data

## Core Idea
A system of record holds the authoritative version of a fact. Derived data is computed from another representation to support a different access pattern or use case. Search indexes, caches, analytical copies, and materialized views are common derived representations.

## Why the Distinction Matters
When data is duplicated, the system must know which copy is authoritative and how the others are updated. A derived view can often be rebuilt from its source, which makes recovery and new views possible. It may also lag behind the source, so readers need an explicit expectation about freshness.

Derivation may transform data rather than copy it exactly. That transformation should have a defined owner and reproducible logic, especially when several systems depend on the result. If the source history is incomplete or external values have changed, a rebuild may not recreate the same result.

## Related Concepts
- [[Materialized Views]]
- [[Event Sourcing]]
- [[CQRS]]
- [[Event Streaming and Log-Based Storage]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1.2 - Trade Off In Data Systems Architecture]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
