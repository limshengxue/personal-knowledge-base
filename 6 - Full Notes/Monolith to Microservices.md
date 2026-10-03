2026-03-01 09:19

Tags: [[software architecture]] [[3 - Tags/kubernetes]]

# Monolith to Microservices
## The Legacy Monolith
- Migrating a tightly coupled monolith can be difficult; a modular monolith can still run successfully in the cloud.
- Build and startup times can grow with application size, but depend on architecture, tooling, and workload.
- Resource needs vary. Monoliths do not inherently require expensive single-machine hardware and may be replicated horizontally.
- Scaling single feature is almost impossible, but only scaling the entire application
- Downtime and maintenance window has to be planned, mitigation for this introduce challenge to keep the instances in sync

## The Modern Microservice
- System composed of individual processes that communicate with each other using API
- Can be deployed individually on separate servers - only required host and what is required by the individual service
- Aligned with Event-driven architecture and Service-Oriented architecture
- Allow different service to be written in the most suitable programming language
- Independent deployments can reduce the scope of outages, but do not guarantee zero downtime; state changes, dependencies, and rollout strategy still matter.
- *However, it adds complexity to administration*

## Refactoring
- Incremental refactoring is often required
- Decision to made:
	- Which business components to separate from the monolith
	- How to decouple databases from application
	- How to test microservices and their dependencies

### Challenges
- Legacy programming language 
- Poorly designed legacy application
- Choosing runtimes (multiple modules on single server introduce conflict)
- Containers solve this problem, ensure application portability with encapsulated lightweight runtime environment


Choose boundaries around cohesive business capabilities, not simply the smallest possible services. [[Cohesion and Coupling]] and [[Single Responsibility Principle]] help assess boundaries; [[Container Orchestration]] helps operate deployments but does not repair poor decomposition.

# References
[[1 - From Monolith to Microservices]]
