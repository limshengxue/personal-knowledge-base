Tags: [[3 - Tags/database|database]] [[3 - Tags/software architecture|software architecture]]

# Separation of Storage and Compute

## Core Idea
Separating storage from compute lets processing instances be replaced or resized without treating their local disks as the durable home of data. A cloud architecture can keep persistent data in a storage service while compute instances handle queries and transformations.

## Trade-offs
Independent scaling helps workloads whose storage and processing needs change at different rates. Shared storage can also support multiple compute workers without copying every data file to each one.

The separation makes the network part of the storage path. Remote reads and writes can add latency and expose the application to network or storage-service failures. Caching and local temporary disks can reduce repeated reads, but the system still needs a clear recovery path when an instance disappears.

Cloud versus self-hosting is a separate economic and operational choice. Predictable demand and existing hardware can favor self-hosting; variable demand can favor elastic cloud capacity. Costs, available skills, visibility into failures, and lock-in also matter.

## Related Concepts
- [[Scalability and Load]]
- [[Reliability and Fault Tolerance]]
- [[Data Warehouses]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1.2 - Trade Off In Data Systems Architecture]]
