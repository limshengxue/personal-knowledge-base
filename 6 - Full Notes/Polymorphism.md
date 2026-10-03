2026-10-03 15:43

Tags: [[software architecture]]

# Polymorphism
- Polymorphism lets the same operation select different behaviour according to the object receiving it.
- Callers depend on a shared contract rather than branching on every concrete implementation.
- [[Abstraction]] defines what callers can use; polymorphism supplies interchangeable implementations.

## Runtime Dispatch
- In Java, an overridden instance method is selected using the receiver's runtime type, even when the reference has an interface or superclass type.
- An interface can support polymorphism without sharing implementation through a superclass.
- Method overloading is different: Java chooses among overloaded signatures at compile time.

## Duck Typing
Python can call an operation on unrelated classes that provide the required behaviour:

```python
class ConsoleRecorder:
    def record(self, message):
        print(message)


class MemoryRecorder:
    def __init__(self):
        self.messages = []

    def record(self, message):
        self.messages.append(message)


def save_event(recorder, message):
    recorder.record(message)


save_event(ConsoleRecorder(), "Order created")
memory_recorder = MemoryRecorder()
save_event(memory_recorder, "Order created")
```

- Both objects support `record(message)`; neither needs to inherit from a common application class.
- Matching a method name is not enough: arguments, results, errors, and side effects must also meet the caller's expectations.

## Design Boundary
- Document the behaviour each implementation promises.
- A replacement that strengthens preconditions or breaks expected results violates the [[Liskov Substitution Principle]], despite compiling successfully.
- Prefer a small contract over a hierarchy created only to share a few lines of code.

# References
[[2 - Source Materials/Course/设计模式之美/2 - Object Oriented Programming|2 - Object Oriented Programming]]

