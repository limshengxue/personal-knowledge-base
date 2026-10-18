Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]]

# Graph Data Models

## Core Idea
A graph represents entities as vertices and relationships as edges. It is useful when queries follow several relationships and the connection pattern matters as much as the entities themselves, as in social, transport, or web networks.

## Representations
A property graph gives vertices and edges identifiers, labels, and properties. A triple store expresses each statement as `(subject, predicate, object)`; the object may be a value or another entity. RDF is a common triple-store model, queried with SPARQL. Cypher expresses patterns and variable-length paths in property graphs.

Graphs can also be stored in relational tables for vertices and edges. Recursive SQL can traverse those tables, though path-oriented queries may be more cumbersome. A graph database is therefore a choice about representation and query ergonomics, not the only way to store connected data.

## Modeling Multi-Party Events
A conventional edge connects two vertices. Three binary edges between a customer, product, and store do not establish that all three took part in the same purchase. Model the purchase as its own vertex and connect all participants to it; date, quantity, and price can then belong to that event. Hypergraphs allow one edge to connect multiple vertices directly, but are less common.

## Related Concepts
- [[Relational vs Document Data Models]]
- [[Declarative Query Languages]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2 - Data Models and Query Languages]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
