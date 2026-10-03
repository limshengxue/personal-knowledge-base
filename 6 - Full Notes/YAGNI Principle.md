2026-10-03 14:19

Tags: [[software architecture]]

# YAGNI Principle
- YAGNI stands for **You Ain't Gonna Need It**.
- Implement capabilities when there is a concrete requirement rather than because they might be useful someday.
- Defer speculative work until the need and its actual shape are clearer.

## Examples of Speculative Work
- Adding a second storage-provider implementation when the application only needs one.
- Creating configuration options for behaviours that no caller currently needs to vary.
- Introducing a dependency for a feature that has not been requested.
- Building a plugin system or abstract base class solely to accommodate imagined future extensions.
- Each addition should have a reason tied to the application's requirements.

## Costs of Unused Capabilities
- They require implementation, tests, documentation, and maintenance.
- Their interfaces and configuration introduce more choices for developers to understand.
- They can constrain later changes around assumptions that turn out to be wrong.
- Time spent on speculative features delays work that already has a demonstrated purpose.

## YAGNI vs KISS
- YAGNI concerns whether a capability needs to be built.
- The [[KISS Principle]] concerns how understandable and maintainable its implementation is.
- A simple implementation can still be unnecessary; a required feature can still be implemented with excessive complexity.
- Apply both: choose necessary work and implement it clearly.

## Keep the Design Adaptable
- Maintain clear boundaries and readable code so changes remain manageable.
- Refactoring, tests, and work needed to satisfy known constraints can support current requirements without adding speculative features.
- Consider future changes when making design decisions, but distinguish preparing a useful boundary from implementing every possible extension.
- An abstraction has a maintenance cost too; introduce it when it solves a concrete problem or protects a justified boundary.
- Revisit a deferred capability when a real requirement emerges rather than treating deferral as a permanent rejection.

# References
[[2 - Source Materials/Course/设计模式之美/13 - KISS and YAGNI Principle|13 - KISS and YAGNI Principle]]
