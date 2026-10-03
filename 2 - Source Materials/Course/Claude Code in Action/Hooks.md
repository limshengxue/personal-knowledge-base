2026-03-14 14:54

# Hooks
- Run a command before or after Claude code does something
- Optionally, blocks its actions
- PreToolUse and PostToolUse hooks - run before and after a tool use
	- PreToolUse can used to block tool call
	- PostToolUse is to provide additional feedback
- We can ask claude for the tool it has access to
- There are more hooks like 
	- Notification
	- Stop
	- SubagentStop
	- PreCompact
	- UserPromptSubmit
	- SessionStart
	- SessionEnd

## Some useful hooks
- Run `tsc` after editing typescript file
-  Run a separate instance to evaluate the current action


# References
