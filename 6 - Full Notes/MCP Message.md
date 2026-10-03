2025-11-02 09:25

Tags: [[gen ai]] [[mcp]]

# MCP Message
## Overall Communication Flow
1. User interaction: User interact with *Host*, expressing an intent or query
2. Host processing: *Host* process user input, potentially using LLM to understand the request and determine which external capabilities might be needed
3. Client Connection: *Host* directs its *client* component to connect to appropriate *server*
4. Capability Discover: *Client* queries *Server* to discover what capabilities (Tools, Resources, Prompts) it offers
5. Capability Invocations: Based on user's needs or LLM's determinations, Host instructs *Client* to invoke specific capabilities from the *Server*
6. Server Executions: The *Server* executes the requested functionality and returns results to *Client*
7. Result Integration: The *Client* relay the result to *Host*, which incorporates them into the LLM context or presents directly to user

## JSON-RPC
- MCP uses JSON-RPC 2.0 as the message format for communication between Clients and Servers
- Lightweight remote procedure call protocol encoded in JSON
	- Human readable and easy-to-debug
	- Language agnostic
	- Well-established, with clear specifications and widespread adoption

### Message Types
- MCP Specs define the full list of valid message types in TypeScript (anyone can access in github repo)
- Request - Result Message
	- Message types where we make a request and expect to get a response back
- Notification message
	- Inform client or server about some event
![[Attachments/Pasted image 20251026093144.png]]

# References
[[3 - The Communication Protocol]]
[[4 - JSON Message Types]]
