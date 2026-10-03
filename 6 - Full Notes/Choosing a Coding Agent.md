2026-10-03 15:43

Tags: [[agentic ai]] [[gen ai]]

# Choosing a Coding Agent
- Choose an agent by how it performs your repository's tasks under your constraints.
- Distinguish the model from the agent harness: tool access, context handling, permissions, and integrations also affect results.
- Treat the course's product characterisations as historical impressions, not permanent rankings.

## Documented Capabilities
Checked against official documentation on 2026-10-03:

| Agent | Capabilities worth evaluating |
| --- | --- |
| Codex CLI | Local repository inspection and editing, command execution, review, skills, MCP integration, and non-interactive workflows |
| Claude Code | Repository work, skills and hooks, specialist subagents, programmatic execution, and experimental agent teams |
| Gemini CLI | Terminal workflows, context management, MCP integration, extensions, skills, and automation |

These are capability descriptions, not evidence that one product is more accurate. [Codex CLI](https://learn.chatgpt.com/docs/codex/cli), [Claude Code documentation](https://code.claude.com/docs/en/sub-agents), [Gemini CLI documentation](https://geminicli.com/docs/).

## Evaluation Checklist
- Task quality: correctness on representative bugs, refactors, and tests.
- Context: relevant-file discovery and adherence to repository instructions.
- Control: supported [[Coding Agent Permissions and Sandboxing]] on your operating system.
- Integration: editor, CI, MCP, and team workflow requirements.
- Operations: latency, usage limits, cost, authentication, and data-handling constraints.

Verify current versions and plans instead of hard-coding prices, context sizes, or model rankings into the decision.

## Small Repository Trial
1. Give each candidate the same bounded tasks and acceptance criteria.
2. Use equivalent repository state and appropriate permissions.
3. Record failures, review effort, test results, and usage.
4. Choose the tool with the best observed fit, not the longest feature list.
5. Re-evaluate when the repository or product changes.

This evaluation procedure is a practical recommendation, not a benchmark result.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/6 Selecting Coding Agent|6 Selecting Coding Agent]]

