Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]]

# Normalization and Denormalization

## Core Idea
Normalization keeps a fact in one authoritative place and refers to it where needed. Denormalization copies or precomputes facts to make particular reads cheaper. The trade-off is between update consistency and query work, not a universal rule that one is faster.

## Example
Store a user's country as an identifier pointing to one country record. If the name changes, it is updated once; queries needing the display name perform a join or lookup. Copy the name into every user record and the read may be simpler, but a change requires updating every copy or tolerating inconsistency.

The same pattern applies to materialized timelines: precomputed membership makes reads cheap while posts, follows, and deletions require propagation. Some denormalized fields are stable enough to copy safely; rapidly changing fields may be better fetched by identifier at read time. Fetching the underlying records for a bounded set of identifiers is sometimes called *hydration*.

## Practical Use
Ask which facts change, which queries need them, how many copies would exist, and what stale results the application can tolerate. If data is duplicated, define the source of truth and the repair path.

## Related Concepts
- [[Relational vs Document Data Models]]
- [[Materialized Views]]
- [[System of Record and Derived Data]]
- [[Star and Snowflake Schemas]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2 - Data Models and Query Languages]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
