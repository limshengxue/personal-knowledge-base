2026-04-25 10:47

Tags: [[agentic ai]]

# Agent Hooks
- Paradigm shift from "user driven" to "event driven"

## Claude Code Lifecycle
- CC provide several events for us to Hook to
- `SessionStart`/`SessionEnd` 
	- Conversation-level
	- Suitable for initialize or cleanup tasks
- `UserPromptSubmit`
	- Suitable to preprocess or validate the prompt
- `PreToolUse`
	- Suitable to provide most granular permission control (with the tool name and arguments of the tool use)
- `PostToolUse`
	- Suitable for automated "wrap up" like lint check
- `Notification`
	- Suitable to notify user
- `Stop/SubagentStop`
	- Ended a roundtrip
![[Attachments/Pasted image 20260425104742.png]]

## Blocking vs Feedback
- Use `PreToolUse` when the decision must happen before an action.
- A command hook can return a structured denial to stop the attempted tool call.
- `PostToolUse` runs after the action, so it cannot undo or prevent that completed action. It can report problems and guide the next step.
- Exit-code and JSON behaviour are event-specific; not every event supports blocking. [Hook reference](https://code.claude.com/docs/en/hooks).

Example output from a `PreToolUse` command hook that rejects an edit:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "This file is outside the approved edit scope."
  }
}
```

This is hook output, not the settings used to register the hook.

## Practical Applications
- Before a tool call: validate paths or reject an operation outside the approved scope.
- After an edit: format the changed file or run a focused compiler check, then report actionable failures.
- On notification: alert the user when input is needed.
- Before finishing: check agreed completion criteria without creating an endless retry loop.

## Operating Safely
- Parse the event's structured input rather than guessing from console text.
- Quote paths, handle failures, and bound execution time.
- Restrict matchers so expensive checks do not run after unrelated events.
- Review hook scripts like executable code. Claude Code's shell sandbox does not automatically contain hooks; see [[Coding Agent Permissions and Sandboxing]].

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/13 Hooks - Automated Action|13 Hooks - Automated Action]]
[[2 - Source Materials/Course/Claude Code in Action/Hooks|Hooks]]
