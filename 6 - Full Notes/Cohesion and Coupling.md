2026-10-03 14:42

Tags: [[software architecture]]

# Cohesion and Coupling
- **Cohesion** describes how closely the responsibilities within a component belong together.
- **Coupling** describes the dependencies between components and how strongly those dependencies tie them together.
- Aim for high cohesion and loose coupling: group related behaviour and keep external dependencies clear and limited.

## High Cohesion
- A cohesive component has a purpose that explains why its data and operations belong together.
- Related methods work on related data and participate in the same concern.
- Low cohesion can appear when different groups of methods use unrelated attributes or serve independent business purposes.
- Evaluate cohesion at the relevant level: a function, class, module, or service.
- Size alone does not determine cohesion. A small class can mix responsibilities, while a larger class can implement one coherent concern.

## Loose Coupling
- Components necessarily collaborate, but those collaborations should rely on explicit contracts rather than unnecessary implementation knowledge.
- Tight coupling appears when a change to one component repeatedly forces changes in others.
- Depending on another component's internal structure, provider-specific types, or undocumented behaviour can strengthen that coupling.
- A stable interface can isolate implementation changes when it expresses the consumer's actual needs.
- Adding an interface does not automatically reduce coupling if the contract still exposes the same implementation details.

## Example: Serialization and Deserialization
- Both operations implement one data format and share its protocol identifier and encoding rules.
- Keeping them in one implementation can preserve cohesion and avoid duplicated protocol knowledge.
- A client that only serializes can depend on a serializer interface; a client that only deserializes can depend on a deserializer interface.
- The implementation remains cohesive while each client depends on a focused contract.
- Splitting the implementation into separate classes may help when responsibilities evolve independently, but careless splitting can introduce duplicated rules or coordinated updates.

## Assessing Component Boundaries
- Can the component's purpose be described clearly?
- Do its operations and data support that purpose?
- Which changes should stay inside the component, and which currently affect its consumers?
- Does the public contract expose details that consumers do not need?
- Would moving a responsibility simplify the dependencies or merely add communication between more components?

## Tradeoffs
- Excessive splitting can scatter related behaviour across many classes and increase coordination.
- Combining unrelated responsibilities can make changes harder to understand and contain.
- Prefer boundaries that reflect business responsibilities and make likely changes manageable.
- Maintainability depends on the quality of the boundaries, rather than minimising the number of classes or dependencies in isolation.

# References
[[2 - Source Materials/Course/设计模式之美/15 - LOD for High Cohesive, Loose Coupling|15 - LOD for High Cohesive, Loose Coupling]]
[[2 - Source Materials/Course/设计模式之美/1 - How to judge quality of code|1 - How to judge quality of code]]
