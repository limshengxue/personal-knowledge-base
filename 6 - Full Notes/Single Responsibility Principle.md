2026-10-03 12:58

Tags: [[software architecture]]

# Single Responsibility Principle
- A class or module should have one responsibility: one cohesive concern that gives it a reason to change.
- Keep behaviours that serve the same responsibility together; separate behaviours that change for independent reasons. This supports high [[Cohesion and Coupling|cohesion]].
- A responsibility can require multiple methods. SRP does not mean one method per class.

## Responsibility Depends on Context
- Judge responsibilities through business logic, use cases, and how the code is used.
- A class that is appropriate for one application may need different boundaries in another.
- Start with the current requirements and revisit the design as responsibilities become clearer.

### Example: User Information and Address
- A `UserInfo` class contains identity, contact details, and address fields.
- If the address is only displayed as part of the user's profile, keeping those fields together may be sufficient.
- If shipping also uses addresses and introduces separate validation or formatting rules, extracting an `AddressInfo` class gives that concern its own boundary.
- The decision follows how the address is used and changes, rather than the number of fields alone.

## Signs a Class May Need Splitting
- Methods work on separate groups of attributes, suggesting unrelated concerns.
- The class depends on many unrelated classes or services.
- Its purpose is difficult to describe, leading to vague names such as `Manager` or `Context`.
- Large groups of private helper methods implement a distinct responsibility that could be extracted.
- The number of methods, attributes, or lines makes the class difficult to understand.
- These are investigation signals, not automatic rules. A long class or many private methods do not by themselves prove an SRP violation.

## Avoid Excessive Splitting
- Smaller classes do not necessarily make a system easier to maintain.
- Serialization and deserialization can share one responsibility: implementing a particular data format.
	- Both depend on the same protocol identifier and encoding rules.
	- Splitting them carelessly can duplicate those rules and require coordinated changes in multiple places.
- If separate classes are useful, keep shared protocol definitions in one place, following the [[DRY Principle]].
- Prefer boundaries that make changes easier to understand and contain. Reconsider a split when it adds coordination without separating independent concerns.

# References
[[2 - Source Materials/Course/设计模式之美/8 - Single Responsibility Principle (SRP)|8 - Single Responsibility Principle (SRP)]]
