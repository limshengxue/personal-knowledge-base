2026-10-03 15:43

Tags: [[software architecture]]

# Spring Dependency Injection
- Spring wires beans by supplying the collaborators they declare.
- This is a framework implementation of [[Dependency Injection]], not a replacement for understanding the principle.
- Depending on an interface can support [[Dependency Inversion Principle]] when the interface represents the application's required behaviour.

## Prefer Constructor Injection
The following fragment assumes a registered `MessageSender` implementation:

```java
import org.springframework.stereotype.Service;

@Service
class ReminderService {
  private final MessageSender sender;

  ReminderService(MessageSender sender) {
    this.sender = sender;
  }

  void remind(String message) {
    sender.send(message);
  }
}
```

- Required dependencies are explicit, can be final, and are available when construction completes.
- A bean with a single constructor does not need `@Autowired` on that constructor.
- Setter injection can represent optional or changeable dependencies. Field injection hides requirements and makes direct construction harder. [Autowiring constructors](https://docs.spring.io/spring-framework/reference/core/beans/annotation-config/autowired.html).

## Wiring Through Configuration
- A `@Bean` method can accept dependencies as parameters and pass them into a constructor.
- This keeps third-party classes independent of Spring annotations.
- An interface alone is not a bean: register a concrete implementation.

## Selecting a Candidate
- `@Qualifier` narrows matching candidates at an injection point.
- `@Primary` gives one candidate preference when ordinary single-value injection would otherwise be ambiguous.
- Do not rely on incidental naming to resolve important choices.

## Failure Signals
- Missing candidate: check registration and component-scan boundaries.
- Ambiguous candidate: select intentionally.
- Constructor cycle: reconsider responsibilities instead of hiding the cycle with field injection.

Scope and lifecycle also affect what instance is supplied; see [[Spring Bean Scopes and Lifecycle]].

# References
[[2 - Source Materials/Books/Spring Start Here/3 - Spring Context - Wiring Bean|3 - Spring Context - Wiring Bean]]
[[2 - Source Materials/Books/Spring Start Here/4 - Spring Context - Using Abstractions|4 - Spring Context - Using Abstractions]]

