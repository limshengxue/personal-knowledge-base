2026-10-03 13:17

Tags: [[software architecture]]

# Liskov Substitution Principle
- An object of a subtype should be usable wherever its base type is expected without breaking the behaviour callers rely on.
- Matching a method signature is insufficient: the subtype must preserve the base type's behavioural contract.
- Callers using the base type should not need special cases for particular subtypes.

## Preserve the Base Type's Contract
- **Inputs:** do not strengthen preconditions. A subtype must accept inputs permitted by the base type's contract.
- **Outputs:** do not weaken postconditions. Results must still satisfy what the base type promises.
- **Exceptions:** preserve the promised failure behaviour; do not introduce unexpected failures for previously valid operations.
- **State:** preserve invariants that the base type guarantees.
- Contracts may be expressed through documentation, tests, and established usage as well as code.

## Example: SecurityTransporter
- Suppose `Transporter.sendRequest()` accepts a valid request without requiring application credentials.
- A `SecurityTransporter` subtype may attach optional credentials before forwarding the request, provided it still honours the base contract.
- If it instead throws an authorization exception whenever credentials are missing, it rejects an operation the base type permits.
- This strengthens the precondition: code that works with `Transporter` can now fail when given `SecurityTransporter`.
- Whether credentials are required is a contract decision. Make that requirement explicit in the relevant abstraction rather than imposing it unexpectedly in a subtype.

## Common Violations
- A method promises to sort orders by amount, but a subtype sorts them by date.
- The base contract accepts negative values, but a subtype rejects them.
- A base type promises an operation, but a subtype replaces it with `UnsupportedOperationException`.
- An override changes a documented guarantee even though its parameters and return type remain the same.
- Different implementations are acceptable when their observable behaviour stays within the base contract.

## Checking Substitutability
- Run the same base-type contract tests against each subtype implementation.
- Exercise the implementations through the base type, as callers would.
- Check accepted inputs, boundary cases, output guarantees, expected exceptions, and state changes.
- Tests help reveal violations; passing a limited set of tests does not prove that every part of the contract is preserved.
- When substitution fails, reconsider the type hierarchy or the abstraction's contract instead of adding subtype checks throughout calling code.

# References
[[2 - Source Materials/Course/设计模式之美/10 - LSP, Liskov Substitution Principle|10 - LSP, Liskov Substitution Principle]]
