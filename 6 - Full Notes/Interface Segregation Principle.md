2026-10-03 13:25

Tags: [[software architecture]]

# Interface Segregation Principle
- Clients should not be forced to depend on operations they do not use.
- Design interfaces around the capabilities each consumer needs.
- One implementation can provide several focused interfaces while each client depends only on the relevant one.

## Example: Updating and Viewing Configuration
- A single configuration interface containing update and display methods would expose unrelated capabilities to different consumers.
- Separate those capabilities into `Updater` and `Viewer`:

```java
import java.util.Map;

interface Updater {
  void update();
}

interface Viewer {
  String outputInPlainText();
  Map<String, String> output();
}
```

- `RedisConfig` implements both interfaces because it supports updating and viewing.
- `KafkaConfig` implements only `Updater`.
- `MysqlConfig` implements only `Viewer`.
- `ScheduledUpdater` depends on `Updater`; it does not need display methods.
- `SimpleHttpServer` depends on `Viewer`; it does not need update methods.
- A new configuration type can support viewing without implementing irrelevant update operations.

## Applying ISP to APIs
### Groups of Operations
- Give different consumers contracts containing the operations they need.
- For example, separate registration, login, and profile lookup from administrative deletion operations.
- One service implementation may implement both contracts.
- Keep authorization checks explicit; splitting interfaces alone does not control access.

### Bundled Computations
- A statistics operation that always computes maximum, minimum, average, and percentiles may make clients pay for results they do not need.
- Provide focused operations or a way to request the required calculations when consumer needs differ.
- A combined operation remains useful when results are commonly needed together or share computation.
- Unused fields alone do not automatically make an API inappropriate; assess unnecessary dependencies, work, and changes imposed on clients.

## ISP vs Single Responsibility Principle
- [[Single Responsibility Principle]] concerns whether a class or module combines responsibilities that change independently.
- ISP concerns the operations exposed through the interface a particular client depends on.
- A cohesive configuration class can still provide separate updating and viewing interfaces without splitting its implementation. This preserves [[Cohesion and Coupling|cohesion]] while narrowing client dependencies.

## Avoid Excessive Fragmentation
- Keep related operations together when they serve the same consumer need.
- Do not create an interface for every method automatically.
- Split an interface when consumers need different capabilities or unrelated changes affect them unnecessarily.
- Balance narrower dependencies against the additional interfaces and wiring they introduce.

# References
[[2 - Source Materials/Course/设计模式之美/11 - Interface Segregation Principle (ISP)|11 - Interface Segregation Principle (ISP)]]
