2026-10-03 14:24

Tags: [[software architecture]]

# DRY Principle
- DRY stands for **Don't Repeat Yourself**.
- Give a shared rule or piece of knowledge one authoritative representation so that changes do not require several independent updates.
- Identify what the code means and why it changes before deciding whether repetition should be removed.

## Similar Code, Different Responsibilities
- Username and password validators may initially perform similar checks.
- Their requirements can change independently: username rules concern identity naming, while password rules concern credentials.
- Merging both policies into one validator solely because their code looks alike can couple unrelated requirements.
- Keep separate entry points for independently governed policies.
- Extract shared mechanics, such as checking a length range, when doing so preserves those separate responsibilities.

## Different Code, the Same Rule
- One IP validator may use a regular expression while another parses the address into numeric parts.
- If both implement the same validation policy, the policy is maintained in two places even though the code differs.
- A rule change can leave the implementations inconsistent.
- Consolidate the policy behind one validation operation and define its expected behaviour with tests.
- Separate implementations can still be justified when they intentionally serve different contracts or provide independent verification.

## Redundant Execution
- A workflow may repeat validation or query the same record twice without needing to.
- Fetching a user once can replace an existence query followed by another query to retrieve that user, when the workflow's consistency requirements allow it.
- An authentication flow must still verify the supplied credentials; finding a user record is not sufficient.
- Place validation according to each boundary's contract. Checks may remain necessary when operations have independent callers or receive untrusted inputs.
- Redundant execution is also an efficiency concern. Sharing a rule's implementation and reducing how often it runs are separate decisions.

## Reuse Without Forced Coupling
- Extract cohesive operations that represent shared knowledge.
- Keep generic mechanics separate from the business policies that use them.
- Share protocol constants and format rules when several operations implement the same protocol.
- Avoid a general-purpose helper whose flags and special cases merely hide unrelated behaviours.
- Evaluate whether a change to the shared rule belongs everywhere it is used. If consumers should evolve independently, reconsider the abstraction.

# References
[[2 - Source Materials/Course/设计模式之美/14 - DRY|14 - DRY]]
