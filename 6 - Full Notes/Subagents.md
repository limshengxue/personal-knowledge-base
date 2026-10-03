2026-04-25 10:48

Tags: [[agentic ai]]

# Subagents
Agent will have problem when dealing with a complex task that can be breakdown to 2 individual (potentially conflicting) subtasks
- For example: "refactor `billing` module, optimize its performance and ensure its comply with security principles"
	- Security audit and performance optimization can be conflicting requirement
	- Agent will conflict itself
- Subagent provide benefits of
	- Context window optimization - subagents work on independent, clean context window
	- Separation of concern - each subagent can have their own prompt, tools, and exploration route

## Anatomy of subagent
- The core of subagents
	- Independent context window
	- Specialised system prompt
	- Modularised tool permission 
![[Attachments/Pasted image 20260412115458.png]]

### Defining sub agent
- Similar like Skills, description is crucial for the main agent to discover and invoke the subagent
- `/agents` command allow us to create and manage subagents
- Or we can define using the structure below and store in `./.claude/agents/` or `~/.claude/agents/`
```
---
name: your-sub-agent-name
description: A clear, keyword-rich description of what this agent does and when to use it.
tools: Read, Grep, Glob
model: opus  # Optional: opus, sonnet, haiku, or inherit
---

You are an expert Go security code reviewer. 
This is the System Prompt, the "soul" of the agent.
It defines the agent's personality, goals, and operational procedures.
```

## Invocation of subagent
- Implicit way - let the main agent invoke itself
- Explicit way - mentioned the subagent name in the prompt to the main agent
- Sequential way - allow agent output consume by another agent by instructions



## Focused Design and Iteration
- Give each subagent one clear responsibility, applying [[Single Responsibility Principle]].
- Define when it should be invoked, the files or questions it owns, and what it must not change.
- Include examples and a concrete output contract: findings, evidence, proposed changes, or verified results.
- Start with a generated draft if helpful, then review and refine it using representative tasks.
- Grant only the tools needed; an analysis-only reviewer usually does not need write or deployment authority.
- Keep shared definitions versioned when the project uses version control. [Subagent design](https://code.claude.com/docs/en/sub-agents).

## Example Assignment
“Review the billing calculations for rounding errors. Do not edit files. Return each finding with the affected path, a reproducible input, expected behaviour, and observed behaviour. If no finding is supported, say so.”

This is more actionable than “be a senior expert and improve everything.”

## Coordination Boundaries
- Separate context reduces distraction, not shared-filesystem conflicts.
- A delegated result is evidence to review, not automatically an accepted implementation.
- Explicitly resolve conflicting findings and validate the combined work.
- Independent tasks may run concurrently; tasks depending on previous results should wait.
- For peer messaging and shared team coordination, see [[Agent Teams and Orchestration]].
- Use [[Coding Agent Permissions and Sandboxing]] to separate role instructions from enforced tool authority.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/16 - Subagent|16 - Subagent]]
[[2 - Source Materials/Course/AI原生开发工作流实战/3 Agent Orchestration|3 Agent Orchestration]]
