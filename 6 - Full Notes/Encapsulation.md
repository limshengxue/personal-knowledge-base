2026-10-03 14:52

Tags: [[software architecture]]

# Encapsulation
- Encapsulation places state and the operations governing it behind a controlled boundary.
- Expose the operations callers need while keeping internal representation and changes under the component's control.
- This helps preserve invariants: conditions that must hold for the component to remain valid.

## Example: A Wallet
- A wallet maintains a balance that must not become negative.
- An unrestricted `setBalance()` lets callers assign values without following the wallet's transaction rules.
- Instead, expose `debit(amount)` and `credit(amount)` operations.
	- Validate the amount before changing the balance.
	- Reject a debit when funds are insufficient.
	- Update the balance only after the checks succeed.
- Callers express their intent through these operations instead of repeating balance calculations and validation.
- A read operation can expose the current balance without permitting arbitrary replacement of the internal state.

## Private Fields Are Only a Starting Point
- Access modifiers such as Java's `private` restrict direct access, but the public API still determines which changes are possible.
- A public setter that accepts invalid values can bypass the intended rules despite the field being private.
- A getter returning an internal mutable list lets callers change that list without using the owner's operations.
- Use immutable values, defensive copies, or appropriately restricted views when exposing data.
- Decide whether callers should receive a snapshot or a live view. A read-only view can still reflect changes made by its owner.
- Copying a collection does not necessarily protect mutable objects inside it; consider what references are exposed.

## Encapsulation vs Abstraction
- **Encapsulation** controls access to state and implementation details and channels changes through an intentional boundary.
- **Abstraction** presents the relevant capabilities while leaving unnecessary details out of the caller's model.
- A wallet's debit operation abstracts the calculation while encapsulation prevents callers from bypassing its balance rules.
- Both support an API that is useful without requiring callers to understand or manipulate its internal representation.

## Designing the Boundary
- Choose operations that express domain intent instead of exposing every field through getters and setters.
- Validate state when objects are created and whenever permitted operations change it.
- Keep implementation details private when callers have no reason to depend on them.
- Access modifiers are one mechanism; modules and closures can also provide boundaries around implementation state.
- Match the boundary to the component's role. Data-transfer objects can expose data without needing the same behaviour as domain objects.
- Keep the interface understandable; excessive restrictions and forwarding methods can make ordinary use unnecessarily difficult.

# References
[[2 - Source Materials/Course/设计模式之美/2 - Object Oriented Programming|2 - Object Oriented Programming]]
