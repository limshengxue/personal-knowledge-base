2025-10-05 09:39

# MCP SDK
- Make it easy to developer MCP Server
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
- Provide with a server inspector


# References
