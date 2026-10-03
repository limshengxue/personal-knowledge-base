2026-10-03 15:43

Tags: [[software architecture]]

# Object-Oriented vs Procedural Programming
- Procedural programming organises work around procedures operating on data.
- Object-oriented programming organises related state and behaviour around objects.
- These are design choices, not a ranking of language quality. One application can combine both.

## Different Centres of Organisation
| Question | Procedural approach | Object-oriented approach |
| --- | --- | --- |
| Where does behaviour live? | Functions and procedures | Objects with responsibilities |
| How is state accessed? | Passed into procedures or shared | Controlled through object boundaries |
| How does behaviour vary? | Parameters, functions, explicit branches | Composition and polymorphic contracts |

- A function that transforms CSV rows can be clearer than a class with no meaningful state.
- An order object can be useful when several operations must preserve the same business invariants.

## Common Design Traps
- Public fields and unrestricted setters can turn objects into passive data bags; use [[Encapsulation]] where invariants matter.
- A giant `Utils` class can collect unrelated responsibilities and hide domain behaviour.
- A global `Constants` class can couple unrelated modules. Keep constants close to their owners.
- Data-transfer objects are valid boundaries; not every record needs business methods. Distinguish their role from [[Anemic vs Rich Domain Models]].

## Choosing an Approach
1. Identify the data flow and rules that must remain true.
2. Use simple procedures for straightforward transformations and orchestration.
3. Introduce objects when state ownership, lifecycle, or interchangeable behaviour improves clarity.
4. Prefer [[Composition over Inheritance]] when combining capabilities.
5. Evaluate [[Cohesion and Coupling]], not merely whether the code contains classes.

Object-oriented syntax does not automatically create a good object-oriented design.

# References
[[2 - Source Materials/Course/设计模式之美/3 - Process Oriented Programming|3 - Process Oriented Programming]]
[[2 - Source Materials/Course/设计模式之美/2 - Object Oriented Programming|2 - Object Oriented Programming]]
[[2 - Source Materials/Course/设计模式之美/6 - Anemic Domain Model vs Rich Domain Model|6 - Anemic Domain Model vs Rich Domain Model]]

