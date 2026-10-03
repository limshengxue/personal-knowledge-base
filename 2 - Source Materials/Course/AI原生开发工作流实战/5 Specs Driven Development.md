2026-04-11 13:10

# 5 Specs Driven Development
- In AI-native development, we transform from coder to specs designer, workflow orchestrator, and quality governer
- The most important role is specs designer
- Specs drive the whole autonomous development

## Loss Translation Problem
- Product Manager -> Developer -> Code
- This process full of loss translation
- Documentation serve code, and code is the source-of-truth

### Reverse of Authority
- In SDD, specs became the source-of-truth and code is its product
![[Attachments/Pasted image 20260411131351.png]]
在这场反转中： 
- 维护软件的核心，从“修改代码”，变成了“演进规范”。 
- 调试 Bug 的核心，从“修复错误代码”，变成了“修正产生错误代码的规范或方案”。 
- 技术重构的核心，从“大规模迁移代码”，变成了“基于同一份规范，生成一个全新技术栈的实现”。

在这个过程中，AI Agent 扮演了多个“编译器”的角色： 
1. 需求编译器：将你用自然语言描述的模糊想法，“编译”成一份结构化的、无歧义的需求规范（spec.md）。 
2. 方案编译器：将需求规范与你的技术约束（如使用 Go 语言）相结合，“编译”成一份详尽的技术实现蓝图（plan.md）。 
3. 任务编译器：将技术蓝图，“编译”成一份带依赖关系的、原子化的任务指令集（tasks.md）。 
4. 代码编译器（生成器）：最终，它根据任务指令集，生成最终的可执行代码。


## SDD Workflow
Intent Definition
- Target: Define the WHAT and WHY
- Input: High-level requirement, vague idea
- Core act: Brain-storm, define and explore edge cases, clarify vague idea, define acceptance criteria
- Output: `spec.md`, do not concern about technical implementation

Technical Planning
- Define HOW
- Input: `spec.md` and develop constraints
- Core acts: AI agents design tech stack, architecture, module, API contract
- Output: `plan.md` and other attachment

Task Decomposition
- ACTIONS
- Input: `plan.md` and other attachment
- Core acts: AI agent breakdown plan into actionable items, define dependecies
- Output: `tasks.md`, a TODO list for coding agent

Autonomous Execution
- Input: `tasks.md`
- AI agent execute tasks
- Output: Executable code, test cases, docs.
![[Attachments/Pasted image 20260411132155.png]]

### Why SDD is the Future
- Solve "vagueness" problem, AI don't do guessing according to prompt
- Accelerate iteration - just have to modify specs and regenerate code instead of changing code
- Parallel execution - as `tasks.md` define dependency, allow parallelism
- Specs become living docs


# References
