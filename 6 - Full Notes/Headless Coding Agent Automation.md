2026-10-03 15:43

Tags: [[agentic ai]] [[ci cd]]

# Headless Coding Agent Automation
- Headless mode runs an agent without the interactive conversation interface.
- It allows scripts and CI jobs to supply input, collect output, and check success.
- The agent still has tool access and context; non-interactive does not mean harmless or deterministic.

## Claude Code Examples
The following Bash examples assume Claude Code is installed and authenticated:

```bash
claude -p "Explain the module layout without changing files." --allowedTools "Read" --output-format json

cat sanitized-error.log | claude -p "Summarize the main error categories. Do not run tools." --output-format json
```

- `-p` runs print mode.
- `--output-format json` returns a result envelope with metadata, not merely the requested prose as raw JSON.
- `stream-json` supports event-oriented consumers.
- Check the process exit status and the result content before using the output. [Programmatic execution](https://code.claude.com/docs/en/headless).

## Permissions and Context
- `--allowedTools` pre-approves tools; do not treat it as a complete deny policy.
- Set explicit restrictions through [[Coding Agent Permissions and Sandboxing]].
- A normal headless run can load project instructions, hooks, and configured MCP servers. Inspect repository configuration before automation.
- Do not rely on unattended jobs to answer interactive approval questions.
- Sanitize logs and inputs before sending them to a model.

## Reliable Pipeline Design
1. Define a narrow task and structured output contract.
2. Set time, cost, and retry limits.
3. Validate output syntax and meaning.
4. Separate analysis from changes and publication.
5. Keep human approval for consequential actions.

For deeper integration, the current Claude Agent SDK offers Python and TypeScript APIs. The source's older name “Claude Code SDK” refers to this programmatic direction, not a guarantee that every run is read-only.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/17 Headless Mode|17 Headless Mode]]
[[2 - Source Materials/Course/Claude Code in Action/Claude Code SDK|Claude Code SDK]]

