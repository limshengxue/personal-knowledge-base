2026-10-03 15:43

Tags: [[agentic ai]] [[gen ai]]

# Coding Agent Permissions and Sandboxing
- Permissions decide which tool actions are allowed, blocked, or require approval.
- A sandbox restricts what executing processes can reach.
- Repository instructions shape agent behaviour but do not enforce an operating-system security boundary.

## Separate the Controls
- Prompt: describes intended behaviour.
- Tool policy: authorises an attempted action.
- Filesystem boundary: limits readable or writable paths.
- Network boundary: limits reachable destinations.
- Credentials: determine the external authority available to the process.

A command allowed by tool policy can still fail inside a sandbox. Conversely, a tool outside the sandbox needs its own protections.

## Claude Code Rules
Current rule precedence is **deny, then ask, then allow**, not the ordering recorded in the source. Inspect rules using `/permissions`. [Permission rules](https://code.claude.com/docs/en/permissions).

A restrictive illustrative settings fragment:

```json
{
  "permissions": {
    "allow": ["Read"],
    "ask": ["Edit"],
    "deny": ["Bash"]
  }
}
```

- This is not a complete security policy: other tools, integrations, and settings also matter.
- Review shared project settings separately from private local settings.
- Avoid broad automatic approval for destructive shell commands.

## Sandbox Scope
Claude Code's built-in shell sandbox does not cover every tool. File tools, hooks, and MCP servers need separate controls. Its documented shell sandbox supports macOS, Linux, and WSL2, not native Windows. [Sandbox scope](https://code.claude.com/docs/en/sandboxing).

## Practical Checklist
- Start with the minimum directories, tools, and network access.
- Remove unnecessary secrets from the environment.
- Review downloaded code and repository hooks before running them.
- Keep deployment, publication, and destructive actions behind explicit approval.
- Use [[Agent Checkpointing and Recovery]] for tracked edits, but remember that recovery does not undo every external side effect.

## MCP Tool Approval
Claude Code settings use the plural `permissions` key. Prefer granting an individual trusted tool instead of automatically approving every tool from a server.

```json
{
  "permissions": {
    "allow": ["mcp__playwright__browser_snapshot"]
  }
}
```

Use the actual configured server and tool names. A server-wide rule such as `mcp__playwright` or `mcp__playwright__*` is broader; it may permit navigation, code execution, or other effects depending on that server. Tool approval is not a filesystem or network sandbox for the MCP server process.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/11 Safety Control - Permission and Sandboxing|11 Safety Control - Permission and Sandboxing]]
