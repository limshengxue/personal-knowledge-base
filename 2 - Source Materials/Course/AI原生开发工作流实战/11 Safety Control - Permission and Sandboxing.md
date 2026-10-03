2026-04-12 09:03

# 11 Safety Control - Permission and Sandboxing
## AI Agent Trust Issue
There are several causes for dangerous behaviour for AI Agent
- Vagueness in prompt
- Limited context
- Stochasticity of statistics

## Permission Control
### Permission Control Design Principle of CC
- Smallest permission control by default
- Required permission from user for any dangerous operation 
- User hold the main authority

### 4 Permission Modes
- Provide macro-level control for permissions
- Shift + Tab to switch mode
![[Attachments/Pasted image 20260412090747.png]]

### Permission Rules
- Allow user to have micro-level permission control using `settings.json` file
- Priority: deny > allow > ask
- Rules can be viewed using the `/permissions` command

Examples: File Access
```js
{
  "permissions": {
    "deny": [
      "Read(./.env*)",       // 禁止读取任何.env文件
      "Read(./secrets/**)",    // 禁止读取secrets目录下的所有文件
      "Read(~/.ssh/*)",       // 禁止读取用户ssh密钥
      "Read(//etc/passwd)",     // 禁止读取系统敏感文件
      "Write(./go.mod)",       // 禁止修改go.mod文件
      "Edit(/docs/**)"        // 禁止编辑docs目录下的文件 (相对于settings.json位置)
    ]
  }
}
```

Examples: Bash Commands
```js
{
  "permissions": {
    "allow": [
      "Bash(go:test:*)",      // 总是允许执行 `go test` 及其任何子命令
      "Bash(npm:run:lint)"    // 总是允许执行 `npm run lint`
    ],
    "ask": [
      "Bash(git:commit:*)"   // 执行 `git commit` 时总是询问
    ],
    "deny": [
      "Bash(rm:-rf:*)",       // 绝对禁止 `rm -rf`
      "Bash(git:push:--force)" // 绝对禁止强制推送
    ]
  }
}
```

Examples: MCP and WebFetch
```js
{
  "permissions": {
    "allow": [
      "WebFetch(domain:*.golang.org)",     // 总是允许访问Go官方文档
      "mcp__github"                        // 总是允许使用名为'github'的MCP服务器的所有工具
    ],
    "ask": [
      "mcp__jira__create_issue"            // 调用jira服务器的create_issue工具时询问
    ]
  }
}
```

## Sandboxing
- Permission control prevent the *known unknown*
- While sandboxing prevent the *unknown unknown*
- Provide 2 modes
	- `Sandbox BashTool with regular permissions` - bash tool follow the permission control settings
	- `Sandbox BashTool with auto-allow in accept edits mode` -  any bash command within sandbox boundary will be auto-allowed, should only be used in non-crucial, personal project 
- Sandbox provide 2 isolations
	- Filesystem Isolation 
		- default write permission in working directory
		- read-only permission for system-wide files
	- Network isolation
		- block all request from sandbox
		- only visit whitelist domain in `settings.json` or visit url when allowed by user
![[Attachments/Pasted image 20260412093901.png]]



# References
