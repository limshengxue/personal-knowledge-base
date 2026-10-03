2026-04-25 10:38

Tags: [[gen ai]] [[agentic ai]]

# Agent Skills
- Ensure general capability of AI Agent while solving domain-specific problem
- It is infeasible to fine-tune agent for every domain nor to feed in all this domain-specific knowledge into the `CLAUDE.MD` file
- We need a more sophisticated knowledge encapsulation and invocation method
- Provides
	- Portable - standardize directory structure that can easily shared
	- Composable - agents can combined multiple skills to solve complex problem

## Instruction vs Skill
- Instruction ([[Slash Command]]) - is a command, invoked by user
- Skill - is a declare statement, declaring to the agent that "i can do certain thing", invoked by agent


## Structure of Agent Skills
- The core is a `SKILL.md` file, which require certain standard field
	- `name` - 1 to 64 chars, only lowercase, number and `-`
	- `compatibility` -  to declare dependencies required by the Skill. Eg. `Need Python 3.11+`
	- `license` and `metadata`
- Follow *Progressive Disclosure* principle, the `SKILL.md` shall not go over 500 lines
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
- Solve the limit of context window 
- Level 1 : Load only the name and description of the Skills into the System Prompt, build an index for skills
- Level 2: When an instruction received from user, the agent use the index to search for matching skill. Once matched, the the body of the `SKILL.MD` will be read.
- Level 3: When the body of the `SKILL.MD` refers to other file, read them according to necessity.
![[Attachments/Pasted image 20260425103928.png]]
## Best Practices and Design Philosophy
### [[Single Responsibility Principle]]
- Skill must like function in coding - do one thing and do it well
- Why
	- Increased discovery accuracy - when skill is focused, its description can be more precise
	- More efficient use of the context window
	- Increase composability

### Well Crafted Description
- `description` is the only entry and must be well-written
- Must be clear 
- 反模式（Vague）：`yaml description: For files`。 
- 最佳实践（Clear）：`yaml description: Analyze Excel spreadsheets, create pivot tables, and generate charts. Use when working with Excel files, spreadsheets, or analyzing tabular data in .xlsx format.` 这个描述不仅说明了能力（分析、创建透视表、生成图表），还给出了明确的触发关键词（Excel, spreadsheet, .xlsx），极大地降低了 AI 的“选择困难症”

### Collaboration and Evolution
- We can encourage skill sharing among project/organisation
- Build together like a code repo
- Versioned the skill


# References
