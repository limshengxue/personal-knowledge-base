2026-10-03 13:30

Tags: [[software architecture]]

# Dependency Injection
- Dependency injection (DI) supplies an object's dependencies from outside rather than having the object construct them internally.
- The consumer declares what it needs; application setup or a framework supplies the implementation.
- This separates using a dependency from choosing and creating it.

## Example: Notification and MessageSender
- If `Notification` constructs its own sender, the choice of implementation is embedded in its notification logic.
- Accepting a sender through the constructor lets the caller choose the implementation.

```java
interface MessageSender {
  void send(String cellphone, String message);
}

class Notification {
  private final MessageSender messageSender;

  Notification(MessageSender messageSender) {
    this.messageSender = messageSender;
  }

  void sendMessage(String cellphone, String message) {
    messageSender.send(cellphone, message);
  }
}
```

- Application setup creates a sender and passes it to `Notification`.
- Production can supply an SMS sender; a test can supply a fake that records calls without sending messages.
- The example uses an interface to make implementations interchangeable. DI can also inject a concrete class; an interface is not required.

## Forms of Injection
- **Constructor injection:** supply dependencies when creating the object. This makes required dependencies explicit and allows fields to remain final.
- **Setter injection:** supply or replace a dependency after creation. Ensure required dependencies are present before the object is used.
- **Method injection:** supply a dependency for a particular operation rather than storing it on the object.
- Choose the form according to when the dependency is needed and whether replacement is useful.

## Manual Wiring and DI Frameworks
- Manual wiring creates objects and passes their dependencies explicitly in application setup.
- A DI framework performs this assembly using registrations, configuration, or declarations.
- Frameworks can also manage dependency lifetimes, such as creating one instance per request or sharing an instance.
- DI itself does not determine lifetime or make a dependency a singleton.
- Explicit construction is often enough for a small object graph; a framework can simplify larger graphs.

## DI, IoC, and DIP
- **DI** describes how a consumer receives its dependencies.
- **[[Inversion of Control]] (IoC)** transfers control from application code to an external coordinator, such as a framework invoking callbacks or assembling objects. DI is one way to apply IoC.
- **[[Dependency Inversion Principle]] (DIP)** concerns dependency direction: high-level policy and low-level details depend on abstractions, while abstractions do not depend on implementation details.
- Injecting a concrete implementation uses DI but does not by itself establish DIP.

## Benefits and Tradeoffs
- Replace implementations without changing the consumer's logic.
- Test the consumer with controlled dependencies instead of relying on external services.
- Make collaborators visible through constructor or method parameters.
- Keep assembly logic understandable; excessive registrations and indirection can make dependencies harder to trace.
- Many unrelated constructor dependencies can signal that the consumer combines too many responsibilities.

# References
[[2 - Source Materials/Course/设计模式之美/12 - Inversion of Control (IOC) and Dependency Injection (DI)|12 - Inversion of Control (IOC) and Dependency Injection (DI)]]
