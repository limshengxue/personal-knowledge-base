2025-10-05 09:08

# Introducing MCP
## MCP
- Shifts the burden of tool definitions and execution onto MCP Server
- *Transport Agnostic* - can communicate over different protocols
	- Std IO - Same machine
	- HTTP/Web sockets - for different machines
- *Communication* - specifications define different types of messages that can be exchanged
	- Some common ones are `ListToolsRequest`, `ListTooslResult`, `CallToolRequest`, `CallToolResult`

![[Attachments/Pasted image 20251005091938.png]]


## MCP Server
- Provide access to data or functionality implemented by some outside service
- Who authors MCP Servers
	- Anyone, often the service provider
- How is using an MCP Server vs Calling API
	- Calling API require schema and function implementation by the service user


# References
