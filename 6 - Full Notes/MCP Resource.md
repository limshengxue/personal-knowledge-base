2025-11-02 09:33

Tags: [[mcp]] [[gen ai]]

# MCP Resource
Allow MCP Server to expose data to client
- For example, listing file available in a code repo
- Similar to GET request handlers in typical HTTP Server
- *Host-controlled*: host decide when to call these. Results are primarily used by the host.
- Direct Resource/Static Resource
	- URI doesn't contain any param
	- E.g. get all docs names
- Templated Resource
	- URI contains one or more params
	- Python MCP SDK automatically parse the params
	- E.g. get specific doc

### MCP Server for Resource
```python
@mcp.resource(
    "docs://documents", # uri
    mime_type="application/json",
)
def list_documents() -> list[str]:
    return list(docs.keys())

@mcp.resource(
    "docs://documents/{doc_id}", # uri
    mime_type="text/plain",
)
def get_document(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Document with id {doc_id} not found.")
    return docs[doc_id]
```


### MCP Client for Resource
```python
    async def read_resource(self, uri: str) -> Any:
        result = await self.session().read_resource(AnyUrl(uri))
        resource = result.contents[0]

        if isinstance(resource, types.TextResourceContents):
            if resource.mimeType == "application/json":
                return json.loads(resource.text)
            return resource.text
```



## SDK Version Scope
The code fragments illustrate the source's 2025-era Python SDK interface. Check installed SDK types and callback signatures before reuse; protocol revision and SDK version are separate choices.

# References
[[Resource]]
