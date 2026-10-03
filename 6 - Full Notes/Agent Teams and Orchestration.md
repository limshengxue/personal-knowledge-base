2026-10-03 15:43

Tags: [[agentic ai]] [[gen ai]]

# Agent Teams and Orchestration
- Agent orchestration coordinates separate workers, their dependencies, and the integration of their results.
- A team is useful when a task contains independent investigations or implementation areas.
- More agents do not automatically produce a better result; coordination consumes time, context, and tokens.

## Teams vs Subagents
- [[Subagents]] usually receive focused delegated work and return results to a parent agent.
- Claude Code agent teams have a lead, separate teammate contexts, a shared task list, and direct messaging between teammates.
- Subagents can also run concurrently; parallelism alone is not the distinction.
- Agent teams are currently experimental, so check the installed version and supported workflow. [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams).

## Task Design
1. Define the shared objective and acceptance criteria.
2. Split work by responsibility or independent hypothesis, not arbitrary file counts.
3. Identify prerequisites and integration points before starting workers.
4. Give each worker explicit scope, allowed actions, expected output, and stopping conditions.
5. Assign ownership where concurrent edits could collide.
6. Have the lead reconcile conflicting findings and validate the combined result.

## Example
For a performance investigation, one worker measures database queries, another profiles application code, and a third checks the benchmark setup. Each returns evidence before any proposed fix is selected.

This is better suited to concurrency than three workers editing the same service simultaneously.

## Practical Limits
- Shared files and external resources remain shared even when contexts are separate.
- Prefer a single agent for small, tightly coupled changes.
- Apply [[Single Responsibility Principle]] to worker assignments and [[Coding Agent Permissions and Sandboxing]] to their tools.
- Independent review is valuable only when the reviewer receives clear criteria and can challenge the implementation.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/3 Agent Orchestration|3 Agent Orchestration]]

