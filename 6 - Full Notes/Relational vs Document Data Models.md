Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]]

# Relational vs Document Data Models

## Core Idea
Relational databases represent data in tables of rows and support joins across them. Document databases group related values into nested records. The useful distinction is how the application accesses and changes related data, rather than whether it can store a particular shape at all.

## When Documents Fit
A document naturally represents an aggregate with one-to-many parts, such as a profile and its small set of addresses. If the application usually reads the aggregate together, storing it together can improve locality and avoid joins. Large documents lose that advantage when requests need only a small part or updates repeatedly rewrite the whole record.

Document stores often offer flexible or schema-on-read structures. This helps with heterogeneous records, but applications still need to understand and validate the variants they read. Schema flexibility is not freedom from schema management.

## When Relations Fit
Relations make shared entities and many-to-many relationships straightforward to represent with identifiers, constraints, and joins. They suit data that is queried through several relationships or updated independently. Object-relational mapping tools reduce code for translating between application objects and tables, but the translation still affects schema and query design.

The boundary has blurred: relational databases can store and query JSON, and document databases can offer joins and indexes. Choose using relationship shape, access patterns, consistency needs, and the cost of updating duplicated data.

## Related Concepts
- [[Normalization and Denormalization]]
- [[Graph Data Models]]
- [[SQL vs Application Business Logic]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2 - Data Models and Query Languages]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
