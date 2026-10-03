2025-10-26 09:36

# 5 - Transport
- MCP Transport refer to the way of passing the JSON message between client and server

## Stdio
- Stdio transport refers to Client launches the MCP Server as a subprocess
- Client write to Server stdin
- Server responds by writing to stdout
- *Only suitable when client and server on same machine*

## Transport Scenario
- There are 4 transport scenario
	- Initial Request from Client to Server (e.g. Tool Call)
	- Response from Server to Client
	- Initial Request from Server to Client (e.g. Sampling)
	- Response from Client to Server
- All 4 can be easily handled by stdio but not streamable HTTP

## Streamable HTTP
- Allow *remote MCP server*

### HTTP Communication
- HTTP clients can easily initiate requests to servers (server hosted in a known URL)
- Server can easily respond
- HTTP servers cannot easily initiate communication with a client
	- Client don't have a known URL
- Some MCP requests is hard to handle by HTTP
	- Sampling, List Roots, Progress Update, Logging


### Workaround - SSE
- During initialization, MCP Server send back `mcp-session-id` using *Initialize Notification* and MCP Client must include the `mcp-session-id` in every subsequent request to the Server
- Client can make a request to Server, Server send back SSE Response to create a SSE connection
- The SSE connection allow server to send requests to the client
- When Client send a request, server create a new SSE connection dedicatedly for message related to that request

![[Attachments/Pasted image 20251026102310.png]]

![[Attachments/Pasted image 20251026102445.png]]


### Stateless
- Stateless HTTP is required when we need horizontal scaling
- For example, when a bunch of server instance stay behind a load balancer, we might get the same instance everytime

![[Attachments/Pasted image 20251026103853.png]]

# References
