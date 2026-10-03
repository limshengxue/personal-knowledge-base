2026-10-03 14:04

Tags: [[software architecture]]

# KISS Principle
- KISS stands for **Keep It Simple, Stupid**.
- Prefer solutions that are easy to understand, use, and maintain while meeting the requirements.
- Judge simplicity by the effort needed to understand and change the design, rather than by line count alone.

## Choose Readable Constructs
- Use clear names and familiar language features to make intent visible.
- Choose specialised syntax or techniques when they express the problem more clearly.
- A straightforward prefix check may be easier to understand than a regular expression for the same task.
- A regular expression can still be appropriate for a pattern that would otherwise require complicated parsing logic.
- Consider the knowledge of the people who will review and maintain the code.

## Reuse Suitable Libraries
- Prefer an established library when it already solves the problem well.
- Compare the complexity of custom code with the dependency's API, configuration, and maintenance requirements.
- A large dependency for a trivial operation may add more complexity than it removes.
- Keep the surrounding integration understandable even when the library's internals are complex.

## Optimise When Needed
- Begin with a clear implementation that satisfies correctness and performance requirements.
- Measure bottlenecks before introducing more complex algorithms, caching, or concurrency.
- Assess improvements against the extra effort required to maintain the solution.
- Preserve clarity where possible and make the reason for necessary complexity understandable.

## Use Code Review to Assess Simplicity
- Can a reviewer explain the code's purpose and follow its execution?
- Does each abstraction simplify understanding or isolate useful behaviour?
- Is there an easier approach that still meets the requirements?
- Does the complexity solve a demonstrated problem?
- Simplicity depends on context; review helps expose assumptions that the author may overlook.

# References
[[2 - Source Materials/Course/设计模式之美/13 - KISS and YAGNI Principle|13 - KISS and YAGNI Principle]]
