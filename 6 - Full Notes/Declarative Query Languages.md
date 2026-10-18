Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]]

# Declarative Query Languages

## Core Idea
A declarative query states the result wanted; an imperative procedure specifies the steps to produce it. SQL is declarative in this sense: the database can choose access paths, join algorithms, and execution order while preserving query semantics.

## Why It Matters
Separating the requested result from execution gives an optimizer room to adapt to indexes, data size, and statistics without rewriting application logic. A query can still be inefficient, and the optimizer's choices depend on available statistics and the SQL written, so real workloads need measurement.

Graph languages such as Cypher and SPARQL also express desired patterns or paths. MapReduce sits closer to an explicit computation: the author supplies map and reduce functions, which should be pure so they can run in different orders and on different machines.

## Related Concepts
- [[Graph Data Models]]
- [[PostgreSQL Indexing Strategy]]
- [[SQL Style and Maintainability]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2 - Data Models and Query Languages]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
