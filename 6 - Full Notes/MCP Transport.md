2025-11-02 09:30

Tags: [[mcp]] [[gen ai]] [[networking]]

# MCP Transport
- A transport frames and delivers the JSON-RPC messages described in [[MCP Message]].
- Match the protocol revision, transport binding, and implementation versions; “HTTP” alone does not establish compatibility.

## Revision 2026 07 28
- Standard bindings are stdio and Streamable HTTP; WebSockets require a custom binding.
- Requests carry version and capability metadata per request.
- Streamable HTTP uses POST to one endpoint with a JSON response or request-scoped SSE stream.
- Servers do not initiate standalone JSON-RPC requests in this revision; interactive operations use the supported message patterns.
- The older connection-scoped initialization/session model is not universal current behavior.

## Stdio
- A client launches a local server subprocess.
- Client protocol messages go to the process's stdin; server protocol output goes to stdout.
- Keep diagnostic logs on stderr.
- Paths visible to that server depend on its environment and authority.

## Legacy HTTP Sessions
For revision 2025-11-25 and compatible earlier implementations:
- A server may return `MCP-Session-Id` in the HTTP response containing `InitializeResult`.
- The client sends that header on subsequent requests only when a session ID was returned.
- SSE can carry server requests and notifications; not every response must open a dedicated stream.
- [[MCP Sampling]] and [[MCP Roots]] examples using a session back-channel belong to this legacy model.

## Scaling and Security
- Do not assume that all horizontal scaling requires one specific legacy stateless configuration.
- Routing, state sharing, reconnection, and negotiated protocol behavior determine the architecture.
- Validate authentication, permitted operations, and the actual endpoint before enabling remote access through [[MCP Client Configuration and Integration]].

## Legacy Course Diagrams
These 2025-era diagrams are retained for comparison. Their session/SSE setup must not be treated as the universal current lifecycle; see the versioned descriptions above.

![[Attachments/Pasted image 20251026102310.png]]

![[Attachments/Pasted image 20251026102445.png]]

![[Attachments/Pasted image 20251026103853.png]]

# References
[[2 - Source Materials/Course/Anthropic - MCP Advanced Technique/5 - Transport|5 - Transport]]
[[2 - Source Materials/Course/MCP - HF/3 - The Communication Protocol|3 - The Communication Protocol]]
[Current transport bindings](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports)
[Legacy transport](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports)
