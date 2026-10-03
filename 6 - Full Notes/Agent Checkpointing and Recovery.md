2026-10-03 15:43

Tags: [[agentic ai]]

# Agent Checkpointing and Recovery
- Checkpointing helps recover when an agent follows an unhelpful direction or produces unwanted edits.
- Conversation state and filesystem state are different; choose which one to restore deliberately.
- A checkpoint is not a complete backup of the machine or external systems.

## Claude Code Checkpoints
- A prompt that starts a turn creates a checkpoint; tracked edits come from Claude's file-editing tools.
- Run `/rewind`, or press `Esc` twice with an empty prompt input, to open recovery options.
- Restore code, conversation, or both depending on the mistake.
- Conversation-only recovery keeps the current files.
- Code-only recovery leaves the conversation available for discussing a new approach. [Checkpointing](https://code.claude.com/docs/en/checkpointing).

## What Is Not Captured
- Files changed by shell commands.
- Manual changes or edits from other processes.
- Database mutations, remote API calls, deployments, and other external state.
- A promise that recovery is available indefinitely; session retention and snapshots have limits.

## Recovery Workflow
1. Stop further actions when a mistake is detected.
2. Inspect the affected files and any external side effects.
3. Select the last suitable checkpoint and the intended restore mode.
4. Confirm the resulting files before continuing.
5. Explain the corrected constraint and rerun focused validation.

If an operation changed a database or remote service, use that system's recovery procedure rather than assuming rewind reverses it.

## Checkpoints vs Version Control
- Agent checkpoints support local experimentation within a session.
- Version control provides durable, reviewable history when the repository uses it.
- Filesystem backups protect material outside either mechanism.
- Combine these controls with [[Coding Agent Permissions and Sandboxing]]; preventing a destructive action is better than relying on recovery afterwards.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/12 Checkpointing|12 Checkpointing]]

