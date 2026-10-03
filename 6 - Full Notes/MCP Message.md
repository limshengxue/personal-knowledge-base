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

## Legacy Session Lifecycle
For revision 2025-11-25 and compatible earlier peers:

1. Establish the selected [[MCP Transport]].
2. Client sends an `initialize` request containing its protocol version, implementation information, and capabilities.
3. Server returns its selected version, implementation information, and capabilities using the same request ID.
4. If the client supports that version, it sends `notifications/initialized`.
5. Exchange operational messages only for negotiated features; discover tools, resources, or prompts as needed.
6. Close the connection using the transport's termination procedure.

Capabilities are declarations of supported protocol features, not a universal grant of permission to use them. [MCP lifecycle](https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle).

## Request IDs and Notifications
- Requests contain an `id`; responses identify the request they answer.
- A response contains a result or an error.
- Notifications have no `id` and do not receive a response.

```json
{
  "jsonrpc": "2.0",
  "method": "notifications/initialized"
}
```

## Closing and Failure Handling
- MCP does not define a universal `shutdown` request followed by an `exit` notification.
- For stdio, close the server's input stream and allow the child process to exit.
- HTTP connection and optional session cleanup depend on the transport.
- Handle unsupported versions, failed capability negotiation, timeouts, and disconnected servers explicitly.
- Keep protocol stdout separate from application logging in stdio servers.

## Current Per Request Protocol
- Revision 2026-07-28 replaces the universal connection-scoped initialize handshake with per-request protocol version and capability metadata.
- Servers accept or reject each request against the supplied version; compatibility handling can fall back to a legacy peer.
- Standalone server-initiated JSON-RPC requests belong to the legacy model, not every modern connection.
- Keep the lifecycle example above when explaining old sessions, but do not apply it to a current peer without checking its revision.

# References
[[2 - Source Materials/Course/MCP - HF/3 - The Communication Protocol|3 - The Communication Protocol]]
[[2 - Source Materials/Course/Anthropic - MCP Advanced Technique/4 - JSON Message Types|4 - JSON Message Types]]
[Current protocol versioning](https://modelcontextprotocol.io/specification/2026-07-28/basic/lifecycle)
