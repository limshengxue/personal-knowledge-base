2025-08-17 10:13

# 3 - The Communication Protocol
## JSON-RPC
- MCP uses JSON-RPC 2.0 as the message format for communication between Clients and Servers
- Lightweight remote procedure call protocol encoded in JSON
	- Human readable and easy-to-debug
	- Language agnostic
	- Well-established, with clear specifications and widespread adoption

### Requests
- Includes
	- Id, Method name to invoke, parameters
- Sent from Client to Server to initiate an operation
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "weather",
    "arguments": {
      "location": "San Francisco"
    }
  }
}
```

### Responses
- Sent from Server to Client in reply to a Request
- Include
	- Same id as the Request
	- Either a result (for success) or an error (for failure)
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "temperature": 62,
    "conditions": "Partly cloudy"
  }
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid location parameter"
  }
}
```

### Notifications
- Server to Client to provide updates or notifications about event
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid location parameter"
  }
}
```

## Transport Mechanism
### stdio (Standard Input/Output)
- Used for local communication, where Client and Server run on same machine
- Simple, no network configuration required, securely sandboxed by the OS

### HTTP + SSE (Server-sent Events)
- Remote communication
- Communication happens over HTTP
- SSE to push updates to the Client over a persistent connection
- Recent updates to MCP standard, introduced or refined *Streamable HTTP* - offers more flexibility by allowing servers to dynamically upgrade to SSE for streaming when needed

## The Interaction Lifecycle
### Initialization
- Client request for Exchange protocol version and capabilities
- Server provide response
- Client confirm the intialization via notification message

### Discovery
- Client requests information about available capability
- Server respond with a list of available tools

### Execution
- Client invokes capabilities based on Host's needs
- Sever provide response
- Before response, server optionally provide progress updates

### Termination
- Gracefully closed the connection when no longer needed
- Server acknowledge the shutdown request
- Client sends the final exit message

## Protocol Evolution
- The MCP protocol is designed to be extensible and adaptable
- The initialization phase includes version negotiation, which allow backward compatibility
- Capability discovery enables Clients to adapt to features of each Server offers

# References
