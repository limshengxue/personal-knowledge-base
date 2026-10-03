2026-10-03 13:04

Tags: [[software architecture]]

# Open Closed Principle
- Software entities should be **open for extension** and **closed for modification**.
- Support new behaviour through defined extension points while keeping established logic stable.
- Identify the kinds of change the design should accommodate. No design can anticipate every future requirement.

## Example: Adding Alert Rules
- A single `Alert.check()` method checks request throughput and error counts.
- Adding a timeout rule requires editing that method and potentially adding parameters to every caller.
- As rules accumulate, the method combines independent behaviours and changes repeatedly.

### Refactor into Alert Handlers
- Put each alert rule in an implementation of an `AlertHandler` interface.
- Pass measurements through an `ApiStatInfo` object rather than a growing list of arguments.
- Let `Alert` delegate checking to its registered handlers.

The dispatch structure below uses the source note's `ApiStatInfo` data object:

```java
import java.util.ArrayList;
import java.util.List;

interface AlertHandler {
  void check(ApiStatInfo stats);
}

class Alert {
  private final List<AlertHandler> handlers = new ArrayList<>();

  void addAlertHandler(AlertHandler handler) {
    handlers.add(handler);
  }

  void check(ApiStatInfo stats) {
    for (AlertHandler handler : handlers) {
      handler.check(stats);
    }
  }
}
```

- Implement a new rule in a `TimeoutAlertHandler` and register it during application setup.
- The dispatch loop and existing handlers remain unchanged.
- Delegating through the interface applies [[Composition over Inheritance]]. Each handler owns a focused rule, supporting the [[Single Responsibility Principle]].

## What Still Changes
- A new rule may require additional measurements in `ApiStatInfo` and changes where those measurements are collected.
- Application setup must register the new handler.
- The extension protects the dispatch logic and existing rules; it does not eliminate all edits throughout the application.
- Adding fields is not automatically harmless: consider affected constructors, callers, and any data contracts.
- Passing existing tests helps check compatibility, but does not by itself establish that a design follows OCP.

## Applying OCP in Practice
- Program against interfaces and encapsulate the behaviour likely to vary.
- [[Dependency Injection]] can supply implementations without putting their selection logic inside the consumer.
- Use knowledge of the business to identify likely changes and choose useful extension points. Apply the [[YAGNI Principle]] before building support for hypothetical requirements.
- Introduce abstractions when they simplify expected changes. Extra interfaces and handlers also add complexity; apply the [[KISS Principle]] when assessing whether they make the design easier to understand.
- Existing code still needs modification for bug fixes, refactoring, or requirements outside the chosen extension points.

# References
[[2 - Source Materials/Course/设计模式之美/9 - Open Closed Principle (OCP)|9 - Open Closed Principle (OCP)]]
