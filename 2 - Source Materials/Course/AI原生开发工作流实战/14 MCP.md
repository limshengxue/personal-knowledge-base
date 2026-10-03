2026-04-12 10:23

# 14 MCP
- A way to allow AI Agent interact with external environment in a safety and structured way
- Using RPC mechanism
- We do we need MCP? MCP vs just curl
	- Fragile (control the whole curl command using prompt)
	- Safety (no token/access control)
	- Stateless (curl have no state)
	- Bad discovery (agent no idea what the endpoint provide)
- Host - orchestrator, CC itself
- Server - ability provider
- Client - invisible to the user, created by host to connect to server, 1:1 for all connected server
![[Attachments/Pasted image 20260412102725.png]]


## Key Features
- Service discovery - server provide info about what tools and parameters of the tools
- Responsibility segregation - client provide intent to use the tool while server execute the tool
- Auth decoupling - LLM does not receive any token or credentials but handle by the host

## Prompts MCP
- Other than exposing tool, MCP server can expose prompts
- This prompts will be converted to slash command by CC once connected with the MCP server


# References
