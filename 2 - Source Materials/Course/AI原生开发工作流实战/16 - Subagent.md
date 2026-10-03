2026-04-12 11:45

# 16 - Subagent
- Agent will have problem when dealing with a complex task that can be breakdown to 2 individual (potentially conflicting) subtasks
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
tools: Read, Grep, Glob, Bash(gosec:*)  # Optional
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

## Best Practices
![[Attachments/Pasted image 20260412120739.png]]



# References
