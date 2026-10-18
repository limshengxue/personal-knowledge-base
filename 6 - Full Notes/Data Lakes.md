Tags: [[3 - Tags/database|database]] [[3 - Tags/olap|olap]]

# Data Lakes

## Core Idea
A data lake stores data as files without requiring every input to conform to one relational schema before storage. This can accommodate tables, text, images, and other data used by analytics or machine learning.

## Place in a Data Pipeline
A lake may hold source-like data before it is transformed into warehouse tables. This preserves flexibility for workloads that need formats or computations outside SQL. Flexibility shifts work to discovery, metadata, governance, and downstream interpretation: file storage alone does not make heterogeneous data easy to use.

## Related Concepts
- [[Data Warehouses]]
- [[OLTP and OLAP]]
- [[System of Record and Derived Data]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1.2 - Trade Off In Data Systems Architecture]]
