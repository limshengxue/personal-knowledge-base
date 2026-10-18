Tags: [[3 - Tags/software architecture|software architecture]] [[3 - Tags/database|database]]

# Reliability and Fault Tolerance

## Core Idea
Reliability means a system continues to provide the service users expect when things go wrong. A **fault** is a component behaving outside its specification; a **failure** is the system failing to provide its required service. Fault tolerance prevents selected faults from becoming failures, but every design has limits.

## Failure Sources
- **Hardware faults** include disk, memory, server, or power loss. Redundancy helps when failures are sufficiently independent. Shared power, networking, or location can still cause correlated failure.
- **Software faults** are often systematic. Identical code running on many machines can fail in the same way when an unusual input or state triggers a hidden assumption. Adding replicas alone does not fix this.
- **Human errors** can be reduced with clear interfaces, safe defaults, testing, gradual rollout, observability, rollback, and easy recovery.

A component whose fault causes a service failure is a *single point of failure*. Removing one often adds replicas or alternate paths, but also adds coordination and operational work. Fault injection can test whether a supposedly redundant system actually survives the faults it claims to tolerate.

## Practical Use
State the service requirement first: what must keep working, for how long, and with what data guarantees? Then trace the components it depends on and the faults that could break them. Design and rehearse recovery for the important paths rather than assuming redundancy implies reliability.

## Related Concepts
- [[Scalability and Load]]
- [[Kafka Partition, Brokers, and Replication]]
- [[Monolith to Microservices]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1 - Reliable, Scalable, and Maintainable Applications]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
