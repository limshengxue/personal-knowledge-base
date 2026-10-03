2026-10-03 15:43

Tags: [[agentic ai]] [[software architecture]]

# AI-Native Development Workflows
- An AI-native workflow treats an agent as part of the development process rather than only a source of code suggestions.
- People remain responsible for goals, constraints, review, and consequential decisions.
- The course's progression from assistant to autonomous workflow is a conceptual model, not a guaranteed maturity roadmap.

## Levels of Assistance
- Knowledge assistance: explain concepts and investigate unfamiliar code.
- Editing assistance: suggest or implement focused local changes.
- Agentic execution: inspect files, use tools, edit, and validate within an agreed scope.
- Orchestrated work: combine repeatable instructions, specialist workers, and validation gates.

Choose the simplest level that reliably handles the task.

## A Controlled Workflow
1. Clarify the goal, exclusions, and acceptance criteria.
2. Supply relevant repository context through [[Context Engineering for AI Coding Assistants]].
3. Use [[Specs Driven Development]] when a specification materially reduces ambiguity.
4. Break the work into bounded steps with visible intermediate results.
5. Execute using appropriate [[Agent Skills]], [[MCP Concepts]], or [[Subagents]].
6. Validate changes with tests, inspections, and independent review where useful.
7. Obtain approval before deployment or other consequential external actions.

## Human Responsibilities
- Decide which problem is worth solving.
- Resolve ambiguous requirements and trade-offs.
- Choose acceptable authority through [[Coding Agent Permissions and Sandboxing]].
- Evaluate evidence rather than accepting a confident completion message.
- Maintain the specifications and repository conventions as the system changes.

## Efficiency
- Automate repetitive, well-defined work before increasing autonomy.
- Reduce irrelevant context and coordination overhead.
- Use [[Headless Coding Agent Automation]] for bounded repeatable jobs and [[Agent Teams and Orchestration]] only when decomposition pays for itself.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/4 From Human-Computer Interaction to AI-Native|4 From Human-Computer Interaction to AI-Native]]
[[2 - Source Materials/Course/AI原生开发工作流实战/1 Introduction|1 Introduction]]
[[2 - Source Materials/Course/AI原生开发工作流实战/18 - AI Cooperative Framework|18 - AI Cooperative Framework]]

