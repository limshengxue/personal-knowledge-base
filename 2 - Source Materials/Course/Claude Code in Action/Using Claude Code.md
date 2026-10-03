2026-03-14 13:29

# Using Claude Code
## Instructions
- `CLAUDE.md` - project scope, public
- `CLAUDE.local.md` - project scope, private
- `~/claude/CLAUDE.md` - across all project
- We can use `#` to update `CLAUDE.md`
	- Useful to do this when claude make mistake

## How to get better result
- Plan Mode - Claude will read more file and gather more context
- Enable Thinking - use token like below (with token budget small to large)
	- Think
	- Think more
	- Think a lot
	- Think longer
	- Ultrathink
- Plan for breadth, think for depth

## Control Context
- Click `Esc` to interupt
	- Useful when making mistake, we can then execute `#` to update the instruction file to prevent repeated mistake
- Click `Esc` twice to rewind conversation
- Use `/compact` when the conversation consume many context
- `/clear` to cleanup the context

## Custom Command
- Add custom command in `.claude/commands`

## MCP
- We can add the following JSON instruction to avoid claude asking for permission related to a single mcp in  `.claude/settings.local.json`
```json
{
	"permission" : {
		"allow": ["mcp__playwright"],
		"deny": []
	}
}
```

## GitHub Integration
`/install-github-app` will install claude code app on github
- It create a PR define 2 actions
![[Attachments/Pasted image 20260314144747.png]]


# References
