2026-03-01 09:19

Tags: [[software architecture]] [[3 - Tags/kubernetes]]

# Monolith to Microservices
## The Legacy Monolith
- Moving monolith to cloud is a pain
- Loading, compiling, building times increase with every new update
- Require expensive singe-piece hardware to run
- Scaling single feature is almost impossible, but only scaling the entire application
- Downtime and maintenance window has to be planned, mitigation for this introduce challenge to keep the instances in sync

## The Modern Microservice
- System composed of individual processes that communicate with each other using API
- Can be deployed individually on separate servers - only required host and what is required by the individual service
- Aligned with Event-driven architecture and Service-Oriented architecture
- Allow different service to be written in the most suitable programming language
- Virtually no downtime and service window
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


# References
[[1 - From Monolith to Microservices]]