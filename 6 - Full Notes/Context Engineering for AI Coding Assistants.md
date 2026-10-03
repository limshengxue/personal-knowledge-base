2026-04-25 10:52

Tags: [[agentic ai]]

# Context Engineering for AI Coding Assistants
## CLAUDE.md and AGENTS.md
### Long term memory
- These MD file is for long term memory
- It is like a handbook for a new employee
- Should be used for high-frequency, general, project-scoped instructions

### AGENTS.md
- `AGENTS.md` is a shared instruction-file convention; discovery and precedence are defined by each host, not guaranteed for every agent.
- Claude Code can import shared instructions using `@AGENTS.md` in a nearby `CLAUDE.md`, with Claude-specific guidance below it. The path is relative to the importing file.
```
# 导入通用的AI Agent协作标准
@AGENTS.md

# --- 以下是Claude Code专属的高级指令 ---

## Sub-agent定义
- 当需要进行安全审查时，请调用`security-reviewer` sub-agent。

## Hooks配置
- 在每次代码编辑后，自动运行`gofmt`。
```

### CLAUDE.md
- User instructions are in `~/.claude/CLAUDE.md`; project instructions can be in `CLAUDE.md` or `.claude/CLAUDE.md`. `CLAUDE.local.md` is local project guidance, not shared by default.
![[Attachments/Pasted image 20260411144203.png]]

### How Claude locates CLAUDE.md
#### Upward Recursion Finding
- Claude Code loads discovered `CLAUDE.md` and `CLAUDE.local.md` files from the filesystem root down to the working directory, not just to the Git root. Contents are concatenated; a later file does not automatically replace an earlier file.
- Allow monorepo structure

#### Downward Dynamic Finding
 - When we use `@` or the agent invoke `READ` to load file from sub-directory, their CLAUDE.md will get loaded
 - This is *Context on Demand* design

#### Example
```
/my-monorepo
├── .git
├── .claude/
│   └── CLAUDE.md         # (A) 项目根上下文
├── services/
│   ├── user-service/
│   │   ├── .claude/
│   │   │   └── CLAUDE.md     # (B) user-service 微服务上下文
│   │   ├── main.go
│   │   └── internal/
│   │       └── db.go
│   └── order-service/
│       ├── .claude/
│       │   └── CLAUDE.md     # (C) order-service 微服务上下文
│       └── main.go
└── libs/
    └── shared-utils/
        └── string.go
```


### Lifecycle of CLAUDE.md
- `/init` create the first CLAUDE.md from scratch
- We can use `/memory` or natural language command to instruct Claude to record something to the file

### Best Practices
模板剖析：每一条指令背后的“为什么” 
一份好的 CLAUDE.md 绝不是简单的规则罗列。它的每一部分，都在解决人机协作中的一个具体痛点。 

核心使命与角色设定 
- 内容：你是一位精通Go语言的资深软件工程师... ** 
- 为什么？这是在为 AI 进行角色扮演（Role-Playing）** 设定。它比一句冷冰冰的“开始工作吧”要有效得多。告诉 AI 它是一个“资深工程师”，会隐式地引导它在给出建议时，更多地考虑代码的可维护性、扩展性和最佳实践，而不仅仅是“能跑就行”。 

技术栈与环境 
- 内容：语言: Go (>= 1.25), Web框架: Gin… 
- 为什么？ 这是在锚定 AI 的知识范围，减少技术栈混淆（但不能保证没有“幻觉”）。明确了技术栈，AI 更不容易在你询问 Gin 框架的问题时，给出一段 Echo 框架的代码。明确了构建和测试命令，AI 在后续提议行动时，就会使用 make test 而不是它自己“猜”的 go test ./...，有助于与项目实践保持一致；仍需检查命令和结果。 

架构与代码规范 
- 内容：项目结构: ..., 错误处理: ..., 日志: ... 
- 为什么？ 这是整个模板中确保代码一致性和质量的最核心部分。尤其是那些用 [强制] 标记的规则，是在为 AI 的行为设定明确的协作要求，而不是技术上不可绕过的安全边界。 
	- 错误处理规则：它能减少 AI 生成 if err != nil { return err }这种丢失上下文的坏代码。 
	- 日志规则：它能引导 AI 生成的日志代码符合团队的结构化日志标准，便于后续的日志聚合与分析。 
	- 项目结构规则：它能让 AI 在创建新文件或模块时，自觉地将它们放置在正确的位置

Git 与版本控制 
- 内容：Commit Message规范: ... 
- 为什么？ 这是在统一团队的协作语言。当 AI Agent 为你自动生成 Commit Message 时，这条规则有助于产出符合约定风格的提交消息，让你的 Git 历史看起来整洁、专业，并且可以被 CI/CD 工具（如 semantic-release）自动解析。 

AI 协作指令 
- 内容：[原则] 优先标准库, [流程] 审查优先, [实践] 表格驱动测试… 
- 为什么？ 这是最高级的用法，也是 CLAUDE.md 与普通文档的本质区别。你不再是简单地“告知”AI 知识，而是在“编程”AI 的行为模式和工作流程。 
	- [流程] 审查优先：这条指令定义了 AI 在面对“实现功能”这类复杂任务时的标准操作程序（SOP），要求它先规划、后行动；还需通过审查和权限控制确认执行符合预期。 
	- [实践] 表格驱动测试：这条指令将团队的技术品味（preference）固化成了 明确要求的实践。 
	- [实践] 并发安全：这条指令利用了 AI 强大的知识库，要求它在处理 Go 语言最复杂、最容易出错的并发问题时，进行额外的风险提示和解释，但不能替代并发测试与专家审查。

## Governance vs Working Instructions
[[AI Project Constitutions]] records the project's governing principles. Project instructions explain day-to-day collaboration; neither substitutes for tests, review, or [[Coding Agent Permissions and Sandboxing|enforced permissions]].

## Version and Enforcement Boundaries
Current Claude Code supports `AGENTS.md` through its built-in plugin from v2.1.277; default loading falls back to it when no project/ancestor `CLAUDE.md` or `CLAUDE.local.md` is found. Configuration can instead load both. Verify the installed version and instruction-loading settings; older versions need an explicit import.

Instruction files guide model behavior. A request to run `gofmt` in Markdown is not an installed hook; executable hooks require actual [[Agent Hooks|hook configuration]].

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/8 Context Engineering - CLAUDE.MD and AGENS.MD|8 Context Engineering - CLAUDE.md and AGENS.MD]]
[[2 - Source Materials/Course/AI原生开发工作流实战/9 Context Engineering - constitution.md.md|9 Context Engineering - constitution.md]]
[Claude Code instruction discovery and AGENTS.md support](https://code.claude.com/docs/en/memory)
