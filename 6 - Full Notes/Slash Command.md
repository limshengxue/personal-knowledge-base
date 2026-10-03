2026-04-25 10:43

Tags: [[agentic ai]]

# Slash Command
- Encapsulate repetitive instructions

## Common Built-in Commands
### Context Management
- `/clear` - reset current context and clear screen
- `/compact` - compress current context, suitable during breaks of a single complex task
- `/rewind` - rewind conversation to a historical point

### Environment and Configuration
- `/config` - pull up a visual, interactive configuration setting interface
- `/permissions` - allow whitelist commands
- `/model` - switch model

### Project and Cooperation
- `/init` - generate a CLAUDE.md
- `/memory` - quick edit CLAUDE.md
- `/code-review` - current bundled code-review skill; `/review` appears in older course material.
- `/pr-comments` - removed in v2.1.91; ask Claude to inspect PR comments using the GitHub CLI instead.

### Metadata
- `/help`
- `/status` - get status of Claude
- `/doctor` - check if installation correct
- `/cost` and `/usage` - view token consumption status
- `/feedback` - report bug to Anthropic

## Custom Commands
- Encapsulate workflow to slash commands
- Usually used for several circumstances
	- High frequency operations
	- Standardize
	- Hidden knowledge of the human team

Passing arguments
```
Please analyze and fix the GitHub issue: $ARGUMENTS.

Follow these steps:
1. Use `gh issue view` to get the issue details.
2. Understand the problem described in the issue.
3. Search the codebase for relevant files.
4. Implement the necessary changes to fix the issue.
5. Write and run tests to verify the fix.
6. Ensure code passes linting and type checking.
7. Create a descriptive commit message.
8. Push and create a PR.

Remember to use the GitHub CLI (`gh`) for all GitHub-related tasks.
```

Multiple arguments
```
Please review Pull Request #$1.
The priority for this review is: $2.
Please focus your review on the changes made by author: $3.
```

Defining the skill
```
---
description: 为指定的Go函数生成符合团队规范的单元测试。
argument-hint: [file_path] [function_name]
model: opus
allowed-tools: Bash(go test:*), Write
---

根据我们在 `constitution.md` 中定义的“测试先行”和“表格驱动”原则，为 `$2` 函数编写一份完整的单元测试。

测试文件应该命名为 `$1`。
...
```

Utilising shell commands
```
---
description: 根据当前暂存区的代码变更，生成一条符合Conventional Commits规范的Commit Message。
allowed-tools: Bash(git branch --show-current), Bash(git diff --staged)
---

你是一位Git专家。请根据以下代码变更的diff信息，为我生成一条符合Conventional Commits规范的、高质量的`git commit`消息。

**当前分支:**
!`git branch --show-current`

**暂存区变更 (Staged Changes):**
!`git diff --staged`

请只输出commit message本身，不要有任何额外的解释。
```




## Current Claude Code Scope
The command list above describes Claude Code, not every coding agent. Check `/help` and the installed version; current bundled review functionality is `/code-review`, rather than assuming the historical `/review` command remains available.

Custom commands and [[Agent Skills]] now share an implementation: `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` can both expose `/deploy`. Invocation controls determine whether a skill is user-invoked, agent-invoked, or both.

Command frontmatter and shell expansion are host features. Set `allowed-tools` to the operations actually needed; a command that only reads `git diff --staged` should not request `git add`. Confirm authorization before publishing, pushing, or creating a PR.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/10 Slash Command|10 Slash Command]]
[Current commands](https://code.claude.com/docs/en/commands)
[Custom commands and skills](https://code.claude.com/docs/en/skills)
