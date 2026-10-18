Tags: [[3 - Tags/software architecture|software architecture]]

# GraphQL

## Core Idea
GraphQL lets a client describe the shape of data it wants from an API. A client can request nested fields needed for a screen without requiring a separate endpoint for every response shape. Its graph-like query syntax does not imply that the backend uses a graph database.

## Server Responsibilities
The server translates requested fields into calls to services or database queries. Since clients can submit queries, the server needs boundaries on cost, depth, and access to data. Recursive or expensive traversal is usually restricted. Resolver design matters: a convenient nested query can otherwise trigger many repeated backend lookups.

GraphQL gives clients flexibility as screens change, but the translation layer and query planning become part of the backend's ongoing work.

## Related Concepts
- [[Declarative Query Languages]]
- [[Graph Data Models]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/3.1 Data Models and Query Languages]]
