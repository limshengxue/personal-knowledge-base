2026-10-03 13:55

Tags: [[software architecture]]

# Inversion of Control
- Inversion of Control (IoC) transfers responsibility for coordination from application code to a framework or another external coordinator.
- The coordinator provides an execution structure and invokes application-defined behaviour at extension points.
- Ask what control is being transferred: execution order, callbacks, object creation, or dependency assembly.

## Example: A Test Runner
- A test runner controls which registered tests run and reports their results.
- Developers supply the test logic through `doTest()`; they do not repeat the runner's orchestration in every test.
- The framework calls the developer's implementation when it reaches the extension point.

```java
import java.util.ArrayList;
import java.util.List;

abstract class TestCase {
  final void run() {
    System.out.println(doTest() ? "Test succeeded." : "Test failed.");
  }

  abstract boolean doTest();
}

class TestRunner {
  private final List<TestCase> testCases = new ArrayList<>();

  void register(TestCase testCase) {
    testCases.add(testCase);
  }

  void runAll() {
    for (TestCase testCase : testCases) {
      testCase.run();
    }
  }
}
```

- Application setup registers concrete subclasses of `TestCase`.
- Each subclass implements `doTest()` with its own checks.
- Once `runAll()` starts, the runner controls iteration and each test's `run()` method invokes the supplied behaviour.
- IoC still allows application code to start and configure the coordinator.

## Library vs Framework
- With a typical library, application code controls the flow and calls library functions when needed.
- With a framework, application code supplies handlers, callbacks, or subclasses that the framework invokes according to its lifecycle.
- The distinction concerns who coordinates the relevant workflow; a system may contain both styles.
- Inheritance is one way to expose extension points. Interfaces and callbacks can support IoC as well.

## IoC vs Dependency Injection
- IoC is the broader idea of transferring control to another coordinator.
- Dependency injection applies it to supplying dependencies: a consumer receives its collaborators instead of selecting and constructing them internally.
- A DI framework can control object creation, assembly, and lifetimes.
- A callback-based framework can invert execution control without providing a DI container.
- Manual dependency injection also transfers dependency assembly to external application setup.

## Benefits and Tradeoffs
- Reuse common orchestration while keeping application-specific behaviour focused on extension points.
- Apply consistent lifecycle and execution rules across implementations.
- Understand registration, callback order, and lifecycle rules when tracing execution.
- Keep extension points clear; indirect control flow can make debugging harder when the coordinator's behaviour is hidden.

# References
[[2 - Source Materials/Course/设计模式之美/12 - Inversion of Control (IOC) and Dependency Injection (DI)|12 - Inversion of Control (IOC) and Dependency Injection (DI)]]
