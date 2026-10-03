2025-11-02 09:17

Tags: [[gen ai]] [[mcp]]

# MCP Concepts
## MCP
- "USB-C" for AI applications - consistent protocol for linking AI models to external capabilities
- Shifts the burden of tool definitions and execution onto MCP Server
- *Transport Agnostic* - can communicate over different protocols
	- Std IO - Same machine
	- HTTP/Web sockets - for different machines
- *Communication* - specifications define different types of messages that can be exchanged
	- Some common ones are `ListToolsRequest`, `ListTooslResult`, `CallToolRequest`, `CallToolResult`

## The Problem it is Solving
- M x N integration problem
- without MCP, connecting model and tool create *M x N problem*
![[Attachments/Pasted image 20250817094116.png]]
- MCP transforms this into a M+N problem by providing standard interface
![[Attachments/Pasted image 20250817094158.png]]


## Components
- Host 
	- the user-facing AI application that end users interact with directly
	- Examples: Anthropic Claude Desktop, Cursor
	- Initiate connections to MCP servers via MCP Client
	- Orchestrate the overall flow between user requests, LLM processing, external tools
	- Render results in a coherent format
- Client
	- A component within the host application that manages communication with a specific MCP server. 
	- Each client maintain 1:1 connection with server
	- Handle protocol-level details of MCP communication 
- Server
	- External program or service that exposes capabilities via the MCP protocol


# References
[[Introducing MCP]]
[[MCP Client]]
[[1 - Key Concepts and Terminology]]
[[2 - Architectural Components of MCP]]
