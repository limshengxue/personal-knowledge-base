2026-10-03 15:43

Tags: [[software architecture]]

# Spring Framework Overview
- Spring Framework provides infrastructure for Java applications so business code need not implement every integration mechanism.
- Its IoC container creates and connects managed objects, called beans. This applies [[Inversion of Control]] through [[Dependency Injection]]. [Spring container overview](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html).

## Main Responsibilities
- Object management: registration, dependency wiring, scopes, and lifecycle.
- Cross-cutting behaviour: aspects can separate infrastructure concerns from business methods.
- Web applications: Spring MVC routes servlet-based HTTP requests to controllers.
- Data access and transactions: supporting infrastructure around database operations.
- Testing: support for exercising components and application contexts.

## Related Projects
- Spring Framework is the foundation; Spring Boot reduces application setup through conventions and auto-configuration.
- Spring Data is a related project family for data-access integration, not another name for the container.
- Add capabilities for the application's actual needs rather than treating the whole ecosystem as mandatory.

## Mental Model
1. Application classes define business behaviour and their dependencies.
2. Configuration tells Spring which objects to manage.
3. The application context assembles the object graph.
4. Web and infrastructure components invoke the managed services.

Read [[Spring Context and Bean Registration]] for setup, [[Spring Dependency Injection]] for wiring, and [[Spring AOP]] for cross-cutting behaviour.

## Adoption Trade-offs
- Useful when common infrastructure, integration, and team conventions outweigh framework overhead.
- A small standalone transformation may need only plain Java.
- Consider startup cost, dependency complexity, deployment constraints, and team familiarity rather than choosing a framework automatically.

# References
[[2 - Source Materials/Books/Spring Start Here/1 - Spring in the Real World|1 - Spring in the Real World]]

