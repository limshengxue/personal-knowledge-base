2025-11-02 09:40

Tags: [[mcp]] [[gen ai]]

# MCP Roots
When MCP Client call some tools that require file access, passing in the file path as argument, server cannot handle it (will throw file not exist)
- If MCP Server and Client is on the same machine, maybe can solve by the user typing in the full path, but this is not user-friendly
- Root can solve this problem

## Grant Permission to Folders/File
- The MCP Server will implement 2 methods, `read_dir` and `list_roots`
- `read_dir` return a list of files/folders in a specific directory
- `list_roots` return a list of folders/files/uri that the server can work on

## Implementation
Client define function that create root object, root object need to be with `file://` according to MCP
```python
    def _create_roots(self, root_paths: list[str]) -> list[Root]:
        """Convert path strings to Root objects."""
        roots = []
        for path in root_paths:
            p = Path(path).resolve()
            file_url = FileUrl(f"file://{p}")
         roots.append(Root(uri=file_url, name=p.name or "Root"))

        return roots
```

Client define method that return root. This method will be called by Server when needed.
```python
    async def _handle_list_roots(
        self, context: RequestContext["ClientSession", None]
    ) -> ListRootsResult | ErrorData:

        """Callback for when server requests roots."""
        return ListRootsResult(roots=self._roots)
```

MCP Server define `list_roots` and `read_dir` tools
```python
@mcp.tool()
async def list_roots(ctx: Context):
    """
    List all directories that are accessible to this server.

    These are the root directories where files can be read from or written to.
    """

    roots_result = await ctx.session.list_roots()
    client_roots = roots_result.roots

    return [file_url_to_path(root.uri) for root in client_roots]

  
  

@mcp.tool()
async def read_dir(
    path: str = Field(description="Path to a directory to read"),
    *,
    ctx: Context,
):

    """Read directory contents. Path must be within one of the client's roots."""

    requested_path = Path(path).resolve()


    if not await is_path_allowed(requested_path, ctx):
        raise ValueError("Error: can only read directories within a root")

    return [entry.name for entry in requested_path.iterdir()]
```

MCP SDK does not promise the file is accessible. We need to check.
```python
async def is_path_allowed(requested_path: Path, ctx: Context) -> bool:
    roots_result = await ctx.session.list_roots()
    client_roots = roots_result.roots


    if not requested_path.exists():
        return False

  
    if requested_path.is_file():
        requested_path = requested_path.parent

    for root in client_roots:
        root_path = file_url_to_path(root.uri)
        try:
            requested_path.relative_to(root_path)
            return True
        except ValueError:
            continue

    return False
```

# References
[[3 - Roots]]