2026-04-25 10:55

Tags: [[agentic ai]]

# AI Cooperative Framework
## Why do we need framework
- Condense experience - a team can have consistent, shared command, skills
- Consistency coding standards and rules under a single repo
- Knowledge can be easily transferred to new members

## Design Principles
- Modularise - each AI capability should be encapsulated into a single document or index
- Layered - leverage both repo level vs personal level
- Shared - the core part of the framework should be shared with Git
- Scalable - can be scaled with new rules and capability

## Framework
![[Attachments/Pasted image 20260425093336.png]]
- The worldview of the AI agent, shaped by
	- `settings.json`, `CLAUDE.md`, `constitution.md` [[Context Engineering for AI Coding Assistants]]
- The capability of the AI Agent defined by
	- `skills` [[Agent Skills]], `commands` [[Slash Command]], `agents` [[Subagents]]
- The automated behaviour of the AI agent defined by
	- `hooks` [[Agent Hooks]]


# References
[[2 - Source Materials/Course/AI原生开发工作流实战/18 - AI Cooperative Framework|18 - AI Cooperative Framework]]
