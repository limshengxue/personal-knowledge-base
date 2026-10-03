2026-04-12 09:58

# 13 Hooks - Automated Action
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
![[Attachments/Pasted image 20260412100245.png]]


# References
