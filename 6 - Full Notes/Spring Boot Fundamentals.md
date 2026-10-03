2026-10-03 15:43

Tags: [[software architecture]]

# Spring Boot Fundamentals
- Spring Boot builds on [[Spring Framework Overview]] to reduce repetitive application setup.
- It supplies dependency management, starter dependencies, and conditional auto-configuration.
- It does not eliminate Spring's beans, dependency injection, or application context.

## What Boot Adds
- Managed dependency versions provide a tested baseline; avoid overriding them without a reason.
- Starters collect dependencies for common application capabilities.
- Auto-configuration responds to the classpath, properties, and existing bean definitions.
- Your own configuration can replace selected defaults; inspect what is actually configured rather than assuming every default applies. [Build systems](https://docs.spring.io/spring-boot/reference/using/build-systems.html), [auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html).

## Application Entry Point
A generated Boot project can use:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
  public static void main(String[] arguments) {
    SpringApplication.run(Application.class, arguments);
  }
}
```

- Place the entry class above the application's packages so default component scanning finds the intended classes.
- `@SpringBootApplication` combines application configuration, component scanning, and auto-configuration.

## Development Workflow
1. Generate a project with Spring Initializr and select only needed dependencies.
2. Add services and controllers under the application package.
3. Keep environment-specific configuration outside business logic.
4. Run and test using the generated Maven or Gradle wrapper.

For a Maven project on Windows, `./mvnw.cmd spring-boot:run` starts development mode and `./mvnw.cmd test` runs its tests. These commands apply to a Boot project, not this Obsidian vault.

A servlet web application can run with an embedded container; Boot is also useful for non-web applications. Read [[Spring MVC Request Lifecycle]] for web request handling.

# References
[[2 - Source Materials/Books/Spring Start Here/1 - Spring in the Real World|1 - Spring in the Real World]]
[[2 - Source Materials/Books/Spring Start Here/7 - Understanding Spring Boot and Spring MVC|7 - Understanding Spring Boot and Spring MVC]]

