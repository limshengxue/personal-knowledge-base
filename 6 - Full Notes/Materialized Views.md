Tags: [[3 - Tags/database|database]] [[3 - Tags/olap|olap]]

# Materialized Views

## Core Idea
A materialized view stores the result of a query or computation so reads do not have to repeat all the work. It trades extra writes, storage, and update complexity for cheaper reads. Unlike an ordinary view, the result is persisted and must be refreshed when source data changes.

## Social Timeline Example
A home timeline can be built by joining followed users to their posts on every read. Alternatively, new posts can be fanned out to followers' precomputed timelines. The second option reduces the cost of common reads but multiplies work per post. An account with millions of followers can overwhelm that path; its posts can be kept separately and merged at read time.

If updates are queued, the view may temporarily lag behind the source. The design must state how much delay is acceptable and how to repair or rebuild a view after a bug or interrupted update.

## Related Concepts
- [[System of Record and Derived Data]]
- [[Scalability and Load]]
- [[CQRS]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
