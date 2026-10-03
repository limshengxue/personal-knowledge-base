2026-10-03 15:11

Tags: [[software architecture]]

# Abstraction
- Abstraction presents the capabilities and concepts relevant to a caller while leaving unnecessary implementation details behind a boundary.
- It reduces how much a caller must understand to use a component correctly.
- Choose what to expose according to the caller's needs; a useful abstraction still defines meaningful behaviour and constraints.

## Example: Retrieving a Picture
- A caller needs a picture URL, but does not necessarily need to know which storage provider holds the picture.
- Expose an operation such as `getPictureURL(pictureId)` and let its implementation handle provider-specific authentication and retrieval.
- The caller depends on the picture-retrieval contract rather than the provider's sequence of operations.
- Renaming `getAwsPictureURL()` alone is insufficient if the parameters, return values, or required setup still expose provider-specific details.
- Specify behaviour the caller needs to understand, such as what happens when a picture is missing or how long a temporary URL remains valid.

## Beyond Interfaces and Abstract Classes
- A function abstracts a sequence of operations behind its parameters, result, and contract.
- A module can expose related capabilities while hiding internal data structures and helper functions.
- Interfaces and abstract classes are language mechanisms for expressing certain abstractions, rather than the definition of abstraction itself.
- A concrete class can also provide a useful abstraction when its public operations hide implementation details.

## Assessing an Abstraction
- Does it express the consumer's problem in terms the consumer understands?
- Can implementation details change without forcing unrelated changes in callers?
- Are inputs, outputs, errors, and relevant constraints clear?
- Does it remove unnecessary knowledge rather than merely moving it into another type?
- Avoid copying an implementation's entire API into an interface when consumers only need a few capabilities.
- Provider-specific operations can be appropriate when the caller actually needs that provider's behaviour. The useful boundary depends on context.

## Abstraction vs Encapsulation
- Abstraction shapes the model and operations presented to the caller.
- Encapsulation controls access to internal state and channels changes through the component's boundary.
- A wallet's `debit()` operation abstracts its balance calculation; restricting arbitrary balance changes encapsulates its state.
- Both help callers use a component without managing its internal representation.

## Tradeoffs
- Extra layers introduce more contracts and indirection to maintain.
- A boundary based on guessed future requirements may be harder to use than a focused solution to the current problem.
- Keep the abstraction as simple as its required behaviour allows and revise it when real consumer needs change.

# References
[[2 - Source Materials/Course/设计模式之美/2 - Object Oriented Programming|2 - Object Oriented Programming]]
[[2 - Source Materials/Course/设计模式之美/4 - Interface vs Abstract|4 - Interface vs Abstract]]
