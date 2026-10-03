2026-10-03 15:43

Tags: [[software architecture]]

# Spring Context and Bean Registration
- An application context is the Spring container used to locate and assemble managed objects.
- A bean definition describes how an object is created; an ordinary object created outside the container is not automatically managed.

## Registration Options
- `@Configuration` with `@Bean`: explicitly construct objects, including third-party classes.
- `@Component` with `@ComponentScan`: discover annotated application classes within selected packages.
- `@Service`, `@Repository`, and `@Controller` communicate specialised component roles.
- Programmatic registration can supply instances through a factory when setup must be calculated.

By default, a `@Bean` method's name becomes the bean name. Its parameters can request dependencies from the container. [Bean declarations](https://docs.spring.io/spring-framework/reference/core/beans/java/bean-annotation.html), [component scanning](https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html).

## Explicit Configuration Example
This requires Spring Context on the classpath:

```java
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class BeanExample {
  @Configuration
  static class AppConfig {
    @Bean
    String greeting() {
      return "Hello";
    }
  }

  public static void main(String[] arguments) {
    try (var context = new AnnotationConfigApplicationContext(AppConfig.class)) {
      System.out.println(context.getBean("greeting", String.class));
    }
  }
}
```

## Identity and Ambiguity
- Names identify bean definitions; types help resolve dependency candidates.
- Multiple beans of the same type need deliberate selection through qualifiers or a primary candidate.
- Use `@Primary`, not the source's `@Default`, for the usual primary-candidate annotation.
- Registration does not imply one instance per Java class; [[Spring Bean Scopes and Lifecycle]] determines instance reuse.
- Keep container lookup at setup boundaries; application services should normally receive dependencies through [[Spring Dependency Injection]].

# References
[[2 - Source Materials/Books/Spring Start Here/2 - Spring Context - Defining Beans|2 - Spring Context - Defining Beans]]

