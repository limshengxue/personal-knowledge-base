2025-08-17 09:52

# 2 - Architectural Components of MCP
## Host, Client, Server
![[Attachments/Pasted image 20250817095521.png]]
### Host
- User facing AI application
- Examples
	- AI chat apps: ChatGPT, Claude Desktop
	- AI enhanced IDE: Cursor
	- Custom AI agents
- Responsibilities
	- Managing user interactions and permissions
	- Initiate connections to MCP servers via MCP clients
	- Orchestrating overall flow between user requests, LLM processing, and external tools
	- Render results in a coherent format

### Client
- Manages communication with a specific MCP server
- Characteristic
	- Maintain 1:1 connection with a single server
	- Handles protocol-level details of MCP communication
	- Act as intermediary between Host's logic and external Server

### Server
- External program or service that exposes capabilities to AI models via MCP protocol
	- Provide access to specific external tools, data sources, or services
	- Act as lightweight wrappers around existing functionality
	- Can run locally or remotely
	- Expose capabilities in standardized format that Client can discover and use

## Communication Flow
1. User interaction: User interact with *Host*, expressing an intent or query
2. Host processing: *Host* process user input, potentially using LLM to understand the request and determine which external capabilities might be needed
3. Client Connection: *Host* directs its *client* component to connect to appropriate *server*
4. Capability Discover: *Client* queries *Server* to discover what capabilities (Tools, Resources, Prompts) it offers
5. Capability Invocations: Based on user's needs or LLM's determinations, Host instructs *Client* to invoke specific capabilities from the *Server*
6. Server Executions: The *Server* executes the requested functionality and returns results to *Client*
7. Result Integration: The *Client* relay the result to *Host*, which incorporates them into the LLM context or presents directly to user




# References
