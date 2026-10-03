2026-03-14 15:31

Tags: [[gen ai]]

# Claude Code Tips
Use this note for practical interactive habits, not a version-independent command reference. Claude Code behavior and available controls change with releases.

## Plan Before Broad Changes
Use Plan Mode to explore and review an approach before implementation. Inspect the selected mode's actual permissions rather than assuming every action is prohibited.

Plan for breadth; request deeper reasoning when a decision needs analysis. Current Claude Code recognizes `ultrathink` as an in-context instruction, not a fixed token-budget tier. “Think”, “think more”, and similar phrases are ordinary prompt text, not an ordered API-budget ladder. Use supported model effort controls when you need an explicit configuration.

## Keep Context Focused
- Press Esc to interrupt a mistaken direction.
- Use `/compact` to summarize a long conversation or `/clear` to start fresh.
- Review [[Agent Checkpointing and Recovery]] before rewinding; conversation recovery is not a backup for external side effects.
- Put durable corrections in [[Context Engineering for AI Coding Assistants|project instructions]] using `/memory` or a deliberate file edit. Do not rely on the historical `#` memory shortcut.
- See [[Slash Command]] and [[Agent Skills]] for reusable workflows.

## Explicit Context Injection
- `@path/to/file` includes a file's content.
- `@path/to/directory` provides a directory listing, not every file's full contents.
- File reads can also bring relevant project instructions into context.

```text
Explain the error handling in @src/auth.py.
Compare the responsibilities in @src/services/ and @src/repositories/.
```

Use narrow references and a specific question rather than loading the whole repository.

## Shell Output as Context
Prefix an interactive input with `!` to run a shell command and add its output to the conversation. For example, `! git status --short` supplies working-tree status.

This executes a command; it is not a request for an explanation. Avoid secret-bearing output, and check [[Coding Agent Permissions and Sandboxing]] because direct user shell input may differ from agent tool calls.

## GitHub Integration
`/install-github-app` can guide repository integration when supported by the environment and your permissions. Review the generated workflow and credential access before enabling automation; the course screenshot is an example, not a guaranteed fixed set of actions.

![[Attachments/Pasted image 20260314144747.png]]

# References
[[2 - Source Materials/Course/Claude Code in Action/Using Claude Code|Using Claude Code]]
[[2 - Source Materials/Course/AI原生开发工作流实战/7 Context Injection and Shell Command|7 Context Injection and Shell Command]]
[Model effort and ultrathink](https://code.claude.com/docs/en/model-config)
[Interactive shell mode](https://code.claude.com/docs/en/interactive-mode#shell-mode-with--prefix)
[File and directory references](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)
[Commands](https://code.claude.com/docs/en/commands)
