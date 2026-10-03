2026-10-03 12:50

Tags: [[software architecture]]

# Composition over Inheritance
- Prefer assembling objects from reusable capabilities when inheritance would force unrelated behaviours into a shared hierarchy.
- Inheritance expresses an **is-a** relationship; composition expresses a **has-a** relationship.
- Delegation means forwarding an operation to an object that implements the required behaviour.

## Problems with Inheritance
- A base class may expose behaviours that some subclasses cannot support.
	- If every `Bird` provides `fly()`, an ostrich would inherit an unsuitable capability.
	- Throwing `UnsupportedOperationException` where callers expect flying breaks the promised behaviour and can violate the [[Liskov Substitution Principle]].
- Deep hierarchies make behaviour harder to trace and changes to base classes harder to assess.
- Different combinations of capabilities can require increasingly complicated subclass hierarchies.

![[Attachments/Pasted image 20260426094231.png]]

## Composition with Interfaces and Delegation
- Define interfaces for capabilities, such as flying, tweeting, and laying eggs.
- Place reusable behaviour in separate implementation classes.
- Give each object only the capabilities it supports and delegate their implementation to its components.
- Interfaces describe what an object can do; composition determines which objects provide that behaviour.

```java
interface Flyable {
  void fly();
}

class WingFlight implements Flyable {
  @Override
  public void fly() {
    System.out.println("Flying with wings");
  }
}

class Sparrow implements Flyable {
  private final Flyable flight;

  Sparrow(Flyable flight) {
    this.flight = flight;
  }

  @Override
  public void fly() {
    flight.fly();
  }
}
```

- `Sparrow` is `Flyable` and has a component that implements flight, such as `new WingFlight()`.
- Another flying bird can reuse the same implementation without inheriting from `Sparrow`.
- An ostrich can compose tweeting and egg-laying behaviours without implementing `Flyable`.
- Passing a component through the constructor applies [[Dependency Injection]]: callers choose the implementation instead of `Sparrow` constructing it internally.

## When to Choose Each Approach
- Prefer composition when capabilities vary independently or implementations need to be interchangeable.
- Inheritance remains useful for a small, stable hierarchy whose subclasses satisfy the base class's contract.
- Framework or library extension points may require subclassing and overriding specific methods.
- Composition introduces extra objects, interfaces, and delegation methods. Avoid adding these layers when a simple design already meets the requirements.

# References
[[2 - Source Materials/Course/设计模式之美/5 - Composition over Inheritance|5 - Composition over Inheritance]]
[[2 - Source Materials/Course/设计模式之美/10 - LSP, Liskov Substitution Principle|10 - LSP, Liskov Substitution Principle]]
