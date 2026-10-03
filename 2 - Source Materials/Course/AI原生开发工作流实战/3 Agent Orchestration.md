2026-04-11 12:36

# 3 Agent Orchestration
### Agent Teams vs Sub-agents
- Sub-agents is by loading different System Prompt to switch personality of the agent
- Sub-agent does not inherit context from Main Agent
- However, it does not support parallel execution

### Agent Teams
- Team Lead + Teammates
	- Team Lead - the Claude interacting with the user
	- Teammates - other Claude instances
- Provide several advantages
	- Completely parallel
	- Independent context
	- Automated cooperation - use mailbox or shared task list

### Best Practices
- Only use when task modularity is present, when tasks are sequential, use single agent
- Token cost will be very high
- Define System Prompt carefully for each member
- Leverage Split Panel (tmux/iTerm2)


# References
