2025-11-02 09:40

Tags: [[mcp]] [[gen ai]]

# MCP Roots
- Roots identify workspace locations a client shares with a server.
- They are informational guidance, not filesystem mounts or access-control grants.
- A root URI does not make a client-only file exist on a remote server.
- Enforce actual authority separately; see [[Coding Agent Permissions and Sandboxing]].

## Client and Server Responsibilities
- The client supplies roots when the negotiated protocol and host support them.
- `read_dir` and `list_roots` in the source are application-defined tools, not mandatory server tool names.
- In the legacy protocol, the server requests roots from the client through the session.
- Revision 2026-07-28 and newer SDK interfaces use different request handling; verify the installed implementation.

## Portable File URIs
Use the path library rather than constructing a Windows URI with string concatenation:

```python
from pathlib import Path

def root_uri(path):
    return Path(path).resolve().as_uri()
```

The path must represent a location the application actually understands.

## Application Containment Check
This illustrates a local policy check, not a complete security boundary:

```python
from pathlib import Path

def is_within_root(requested_path, root_path):
    resolved_request = Path(requested_path).resolve()
    resolved_root = Path(root_path).resolve()
    try:
        resolved_request.relative_to(resolved_root)
    except ValueError:
        return False
    return True
```

- Resolve the requested path and root before comparing ancestry; do not rely on string prefixes.
- Reject nonexistent paths when the operation requires an existing file.
- Validate directory type before listing entries.
- Symlink changes and time-of-check/time-of-use races require additional protection where paths are untrusted.
- A sandbox, credentials, and server-side authorization remain necessary for sensitive operations.
- Configure compatible roots support through [[MCP Client Configuration and Integration]].

# References
[[3 - Roots]]
[SDK roots and compatibility](https://py.sdk.modelcontextprotocol.io/v2/handlers/sampling-and-roots/)
