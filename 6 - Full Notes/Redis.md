2026-03-15 15:55

Tags: [[3 - Tags/redis|redis]]

# Redis
## What is Redis
- Memory first
	- In-memory storage provides unparalleled data access speed
	- Retention depends on expiry, eviction, persistence configuration, and operational failures.
- Key-value data store
	- No-SQL database
	- Support standard data structures like strings, lists, JSON, vectors

## Main Use Cases
### Cache
- In-memory access can provide low latency; end-to-end response time depends on command, network, workload, and deployment.

### Search and query
- Search by text, identifiers, range, or location
- Can be executed on hash or JSON

### Session Management
- Distributed data
- Scale to multiple servers
- Low latency
- Example use case: Shopping session (cart)

### Vector Search
- Recommendation
- Semantic caching
- Anomalies detection

## Redis Products
- Open Source
- Redis Cloud - managed DaaS
- Redis Software - self-managed enterprise deployment
- Redis Insight - allow connecting to Redis database

# References
[[Explore Redis for Developers]]
[[Redis Use Case]]
[Redis Software](https://redis.io/docs/latest/operate/rs/)
