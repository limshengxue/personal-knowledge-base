2026-10-03 18:49

Tags: [[agentic ai]]

# AI Project Constitutions
A project constitution records the enduring principles and constraints that govern development decisions. In [[Specs Driven Development]], it provides a baseline against which specifications, plans, and implementations are reviewed.

## Constitution vs Working Instructions
- **Constitution**: explains WHY a principle matters and which constraints MUST or MUST NOT be violated.
- **Project instructions**: describe HOW to work in the repository, such as commands, file locations, and coding conventions. See [[Context Engineering for AI Coding Assistants]].
- Constitutions usually change less often, through a deliberate amendment process; routine instructions evolve with tooling and project structure.

“Non-negotiable” expresses a governance commitment, not an automatic technical enforcement mechanism. Both kinds of documents can contain mandatory requirements; the distinction is their intended role, not the filename alone.

## Make Principles Reviewable
Write concrete principles with rationale, scope, and an exception process. For example: “New API behavior must have tests demonstrating its acceptance criteria.”

Include a constitution check in `plan.md`, reference the constitution from project instructions, and revisit the check during implementation and review. Record approved exceptions and amendments rather than silently bypassing requirements.

Use tests, CI checks, and [[Coding Agent Permissions and Sandboxing|permission boundaries]] for enforceable controls. Loading a constitution into model context does not prove compliance.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/9 Context Engineering - constitution.md.md|9 Context Engineering - constitution.md]]
[Spec Kit constitution workflow](https://github.com/github/spec-kit)
