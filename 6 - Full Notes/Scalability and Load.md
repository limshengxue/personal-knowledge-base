Tags: [[3 - Tags/software architecture|software architecture]] [[3 - Tags/database|database]]

# Scalability and Load

## Core Idea
Scalability is the ability to cope with increased load while keeping a stated level of service. It is meaningful only when the load and performance target are specified. Requests per second, concurrent users, read/write ratio, and data volume describe different pressures.

## Ways to Add Capacity
- **Scale up:** use a more powerful machine. It keeps coordination relatively simple, but large machines can become disproportionately costly and have finite capacity.
- **Scale out:** distribute work across machines. Shared-nothing nodes offer independent CPU, memory, and storage, but require software coordination and introduce network failure modes.
- **Shared disk:** multiple compute nodes use common storage. This can simplify data access, while contention and locking can limit gains.
- **Elastic scaling:** adjust capacity as demand changes. It depends on the system being able to add and remove capacity safely and quickly enough.

## Design From the Workload
An architecture that improves one operation can shift cost to another. A social timeline can be assembled on every read, or precomputed when posts arrive. Precomputation makes ordinary reads cheaper but multiplies writes and becomes expensive for accounts with many followers. A hybrid path treats those high-fanout accounts differently.

Large systems often scale best when work can be divided into largely independent parts. This is a design direction, not a promise of linear speedup: skew, coordination, and shared resources can still dominate.

## Related Concepts
- [[Response Time and Tail Latency]]
- [[Materialized Views]]
- [[Reliability and Fault Tolerance]]
- [[Concurrency vs Parallelism]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1 - Reliable, Scalable, and Maintainable Applications]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
