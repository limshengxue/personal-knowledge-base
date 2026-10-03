2026-10-03 15:43

Tags: [[software architecture]]

# Spring Bean Scopes and Lifecycle
- Bean scope determines when instances are reused or created.
- Lifecycle describes creation, initialisation, use, and cleanup.
- These are separate decisions: a correctly wired bean can still have the wrong lifetime.

## Common Scopes
| Scope | Instance boundary |
| --- | --- |
| `singleton` | One instance per bean definition per container; the default |
| `prototype` | A new instance each time the container is asked for that bean |
| `request` | One instance per HTTP request in a web-aware context |
| `session` | One instance per HTTP session in a web-aware context |

A Spring singleton is not one instance for the entire JVM. Mutable singleton state also needs its own concurrency strategy; scope does not provide thread safety. [Bean scopes](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html).

## Eager and Lazy Creation
- Application contexts normally initialise non-lazy singleton beans during startup.
- `@Lazy` can defer creation, but a dependency needed by an eagerly created bean can still be initialised.
- A prototype injected into a singleton constructor is resolved once, not recreated on every service method call.
- For fresh instances on demand, declare an `ObjectProvider<Worker>` and call `getObject()` at the intended boundary.

## Lifecycle Callbacks
- Initialisation callbacks run after dependencies have been populated.
- Modern Spring applications use `jakarta.annotation.PostConstruct` and `PreDestroy` when those annotations are available and configured.
- `@Bean(initMethod = "...", destroyMethod = "...")` can describe callbacks without adding Spring interfaces to the object.
- The container does not automatically run prototype destruction callbacks; the caller must manage their cleanup. [Bean lifecycle configuration](https://docs.spring.io/spring-framework/reference/core/beans/java/bean-annotation.html).

Use [[Spring Dependency Injection]] to declare collaborators, then choose scopes that fit their ownership and resource lifetime.

# References
[[2 - Source Materials/Books/Spring Start Here/5 - Spring Context - Bean Scopes and Life cycle|5 - Spring Context - Bean Scopes and Life cycle]]
[[2 - Source Materials/Books/Spring Start Here/2 - Spring Context - Defining Beans|2 - Spring Context - Defining Beans]]

