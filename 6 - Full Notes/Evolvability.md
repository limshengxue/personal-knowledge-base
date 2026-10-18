Tags: [[3 - Tags/software architecture|software architecture]]

# Evolvability

## Core Idea
Evolvability is how easily a system can adapt to new requirements, platforms, and constraints. It depends on whether people can understand its behavior, change a part without unpredictable effects elsewhere, and validate the result.

## Managing Complexity
Some complexity belongs to the problem domain; accidental complexity comes from the implementation or tooling. Useful abstractions and clear boundaries can reduce the latter. Hidden dependencies and assumptions make change risky even when the codebase is small.

Refactoring, testing, and incremental rollout help a team make and evaluate changes. A design should also preserve the reason behind important choices so future maintainers can distinguish an intentional constraint from an accident of history.

## Related Concepts
- [[Abstraction]]
- [[Cohesion and Coupling]]
- [[KISS Principle]]
- [[Operability]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1 - Reliable, Scalable, and Maintainable Applications]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
