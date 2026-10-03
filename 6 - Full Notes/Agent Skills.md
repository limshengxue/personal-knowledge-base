2026-04-25 10:38

Tags: [[gen ai]] [[agentic ai]]

# Agent Skills
- Ensure general capability of AI Agent while solving domain-specific problem
- It is infeasible to fine-tune agent for every domain nor to feed in all this domain-specific knowledge into the `CLAUDE.md` file
- We need a more sophisticated knowledge encapsulation and invocation method
- Provides
	- Portable - standardize directory structure that can easily shared
	- Composable - agents can combined multiple skills to solve complex problem

## Invocation and Persistent Instructions
- [[Context Engineering for AI Coding Assistants|Project instructions]] supply persistent guidance; they are not synonymous with slash commands.
- A [[Slash Command]] is a user-facing invocation mechanism. A skill packages instructions and optional supporting resources.
- Invocation is host-specific: skills can be invoked by users or selected by agents. In current Claude Code, custom commands and skills share an implementation, with controls for user-only or agent-only invocation.


## Structure of Agent Skills
- The core is a `SKILL.md` file, which require certain standard field
	- `name` - required; 1 to 64 chars, using lowercase letters, numbers, and hyphens
	- `description` - required; describes the capability and when to use it
	- `compatibility` -  to declare dependencies required by the Skill. Eg. `Need Python 3.11+`
	- `license` and `metadata`
- Follow *Progressive Disclosure*; keeping `SKILL.md` below 500 lines is a guideline for concise instructions, not a hard format limit.
- The `SKILL.md` can act as a table of content that direct the agent to other file in the directory

```
pdf-processing-skill/
├── SKILL.md          # 技能的入口和核心指令
├── forms.md          # 专门处理PDF表单的详细指南
├── reference.md      # 更深入的PDF处理技术参考
└── scripts/
    ├── fill_form.py  # 可被SKILL.md中指令调用的Python脚本
    └── ...           # 其他辅助脚本
```

## Progressive Disclosure
- Reduces context consumption; does not eliminate context-window limits 
- Level 1 : Load only the name and description of the Skills into the System Prompt, build an index for skills
- Level 2: When an instruction received from user, the agent use the index to search for matching skill. Once matched, the the body of the `SKILL.md` will be read.
- Level 3: When the body of the `SKILL.md` refers to other file, read them according to necessity.
![[Attachments/Pasted image 20260425103928.png]]
## Best Practices and Design Philosophy
### [[Single Responsibility Principle]]
- Skill must like function in coding - do one thing and do it well
- Why
	- Increased discovery accuracy - when skill is focused, its description can be more precise
	- More efficient use of the context window
	- Increase composability

### Well Crafted Description
- `description` is a primary discovery signal and must state both capability and when to use it
- Must be clear 
- 反模式（Vague）：`yaml description: For files`。 
- 最佳实践（Clear）：`yaml description: Analyze Excel spreadsheets, create pivot tables, and generate charts. Use when working with Excel files, spreadsheets, or analyzing tabular data in .xlsx format.` 这个描述不仅说明了能力（分析、创建透视表、生成图表），还给出了明确的触发关键词（Excel, spreadsheet, .xlsx），极大地降低了 AI 的“选择困难症”

### Collaboration and Evolution
- We can encourage skill sharing among project/organisation
- Build together like a code repo
- Versioned the skill


## Deterministic Work and Evaluation
- Put stable transformations, parsing, and validation in scripts when they should behave the same way every time.
- Let the model handle interpretation and decisions instead of repeatedly recreating known algorithms.
- A script is only deterministic to the extent that its inputs, dependencies, and environment are controlled.
- Document prerequisites and failure behaviour, and keep script execution within [[Coding Agent Permissions and Sandboxing]].

### Build an Evaluation Set
1. Collect realistic prompts that should activate the skill.
2. Include similar prompts that should not activate it.
3. Define observable assertions: required outputs, valid schemas, preserved files, and correct edge-case handling.
4. Run cases against the current skill and a baseline without it.
5. Inspect failures separately for discovery, instructions, script behaviour, and final output quality.
6. Revise one part at a time and rerun the same cases.

### Improve the Skill
- Check both task success and unwanted side effects.
- Prefer evidence from repeatable cases over one convincing demonstration.
- Use blinded comparisons when human judgement is needed.
- Keep detailed references outside the main instructions and load them only when needed.
- Name and description are required metadata; compatibility and other fields are optional. The specification recommends concise instructions and supports optional scripts and references. [Agent Skills specification](https://agentskills.io/specification).

The course's grader and comparison examples are evaluation patterns, not mandatory components of every skill.

# References
[[2 - Source Materials/Course/AI原生开发工作流实战/2 Agent Skills Standard and Guidelines|2 Agent Skills Standard and Guidelines]]
[[2 - Source Materials/Course/AI原生开发工作流实战/15 Agent Skills|15 Agent Skills]]
[Claude Code skill invocation](https://code.claude.com/docs/en/skills)
