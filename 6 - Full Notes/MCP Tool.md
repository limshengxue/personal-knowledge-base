2025-11-02 09:33

Tags: [[gen ai]] [[mcp]]

# MCP Tool
- Provide function to the LLM
- *Model-controlled* - execution based on LLM

### MCP Client
```python
    async def list_tools(self) -> list[types.Tool]:
        result = await self.session().list_tools()
        return result.tools

    async def call_tool(
        self, tool_name: str, tool_input: dict
    ) -> types.CallToolResult | None:
        return await self.session().call_tool(tool_name, tool_input)
```

### MCP Server
```python
@mcp.tool(
    name = "read_doc_contents",
    description = "Reads the contents of a document and return it as a string.",
)

def read_doc_contents(
    doc_id: str = Field(description = "Id of the document to read")
    ) -> str:

    if doc_id in docs:
        return docs[doc_id]
    else:
        return ValueError(f"Document with id {doc_id} not found.")
```


# References
[[Tool]]
[[MCP SDK]]
[[MCP Client]]