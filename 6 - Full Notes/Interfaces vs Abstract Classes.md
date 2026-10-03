2026-10-03 14:46

Tags: [[software architecture]]

# Interfaces vs Abstract Classes
- Interfaces define contracts that different implementations can satisfy.
- Abstract classes provide a shared base that can combine a contract, instance state, and reusable implementation.
- Neither can be instantiated directly; concrete classes supply or inherit the implementations required by their contracts.

## Java Comparison

Language rules are documented in the Java Language Specification chapters on [interfaces](https://docs.oracle.com/javase/specs/jls/se25/html/jls-9.html) and [classes](https://docs.oracle.com/javase/specs/jls/se25/html/jls-8.html).

| Aspect | Interface | Abstract class |
| --- | --- | --- |
| Typical purpose | Describe a capability or contract | Share state and implementation within a class hierarchy |
| Instance fields | None; declared fields are `public static final` | Can declare instance and static fields |
| Constructors | None | Can initialise the base portion of subclass objects |
| Method bodies | Supported by `default`, `static`, and `private` methods | Supported by concrete methods; abstract methods have no body |
| Type inheritance | A class can implement several interfaces; an interface can extend several interfaces | A class can extend only one superclass, which may be abstract |

- An implementing class may inherit suitable method implementations; it does not always need to redeclare every interface method.
- Conflicting inherited default methods may require an explicit override.
- Implementing an interface establishes an **is-a** subtype relationship. Holding another object as a component expresses **has-a**.

## Example: Contract and Shared State

```java
interface Named {
  String name();

  default String label() {
    return "[" + name() + "]";
  }
}

abstract class NamedEntity implements Named {
  private final String name;

  protected NamedEntity(String name) {
    this.name = name;
  }

  @Override
  public final String name() {
    return name;
  }

  abstract void process();
}
```

- `Named` supplies a contract and a default operation without storing an instance's name.
- `NamedEntity` stores that state and implements `name()` for its subclasses.
- A concrete subclass must implement `process()` and can inherit both `name()` and `label()`.
- Unrelated classes can implement `Named` without extending `NamedEntity`.

## Choosing an Abstraction
- Use an interface when consumers need a stable capability and implementations should remain free to use different class hierarchies.
- Use an abstract class when closely related subclasses benefit from shared state, construction, or an execution structure.
- Combine them when a public contract and an optional reusable base each serve a useful purpose.
- An abstract base consumes the subclass's single superclass slot and ties it to inherited behaviour.
- Keep contracts focused on consumer needs. Adding a type solely to mirror one implementation's details provides little isolation.
- Weigh the benefits against the extra types and relationships introduced; a simple concrete class can be sufficient.

A contract exposes an [[Abstraction]] and enables [[Polymorphism]] across implementations. Apply [[Interface Segregation Principle]] to keep contracts consumer-focused; consider [[Composition over Inheritance]] before introducing a shared base class.

# References
[[2 - Source Materials/Course/设计模式之美/4 - Interface vs Abstract|4 - Interface vs Abstract]]
