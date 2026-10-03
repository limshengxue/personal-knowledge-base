2025-09-28 10:39

# 6 - MCP Clients
- Crucial components that act as bridge between AI applications and external capabilities provided by MCP Servers

## UI Client
- Chat Interface Client
	- Claude Desktop
- Interactive Development Client
	- VS Code Extension
	- Cursor IDE

## Configuring MCP Client
`mcp.json` structure
```json
{
  "servers": [
    {
      "name": "Server Name",
      "transport": {
        "type": "stdio|sse",
        // Transport-specific configuration
      }
    }
  ]
}
```

Configuring stdio transport include the command and the arguments
```json
{
  "servers": [
    {
      "name": "File Explorer",
      "transport": {
        "type": "stdio",
        "command": "python",
        "args": ["/path/to/file_explorer_server.py"] // This is an example, we'll use a real server in the next unit
      }
    }
  ]
}
```

Remote Server usually include the URL
```json
{
  "servers": [
    {
      "name": "Weather API",
      "transport": {
        "type": "sse",
        "url": "https://example.com/mcp-server" // This is an example, we'll use a real server in the next unit
      }
    }
  ]
}
```


## Sample - Create a Tiny Agent with MCP (web browsing)
```json
{
    "name": "playwright-agent",
    "description": "Agent with Playwright MCP server",
    "model": "Qwen/Qwen2.5-72B-Instruct",
    "provider": "nebius",
    "servers": [
        {
            "type": "stdio",
            "command": "npx",
            "args": ["@playwright/mcp@latest"]
        }
    ]
}
```

Setting of HF token is required (restart terminal)
```

setx HUGGINGFACE_HUB_TOKEN "<YOUR_HF_TOKEN>"
$env:HUGGINGFACE_HUB_TOKEN = "<YOUR_HF_TOKEN>"

npx @huggingface/tiny-agents run agent.json
```


# References
