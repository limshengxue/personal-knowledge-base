2026-04-11 14:20

# 7 Context Injection and Shell Command
## Context Injection
- Use `@` command to inject context into the coding agent
- Inject single file, use cases:
	- Code explanation
	- Test case generation
- Inject a directory (Claude automatically ignore `.gitignore` file), use cases:
	- For project understanding
	- Large-scale code refactor

## Shell Command
- We use `!` to directly execute shell command in the TUI of Claude
- The output of the shell command get injected into its context
- Use cases
	- Verification
	- Generate commit message

# References
