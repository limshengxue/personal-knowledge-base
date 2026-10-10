# Fix the Factory, Not Just the Product - Improving the Coding Agent Harness

Tags: [[3 - Tags/agentic ai|agentic ai]]

2026-10-10 07:53

## Core Idea

把 Vibe Coding 的 **agent harness** 想象成一座生产代码的工厂：模型、提示词、项目指令、Skills、工具、测试和反馈机制共同决定产物质量。遇到纯粹的 coding bug，只修眼前这一个文件是修了「产品」；更长期的做法是调查 **为什么这套系统会犯这个错，以及下次如何避免**。

这并不是说出现 bug 就不该立即修复。正确顺序应该是 **止损 / 修复当前产物 → 验证正确性 → 判断是否存在可复用的系统性改进 → 更新工厂 → 用相同失败案例复测**。

## What Counts as a Factory Defect?

先区分不同性质的问题，才知道能否从 harness 层防止重犯。

- **适合沉淀的错误**：重复出现的代码 bug、违反既定代码规范、遗漏错误处理、错误调用项目专用工具、没有执行约定测试、重复踩已知环境坑。
- **不能简单归罪于 Agent 的情况**：需求后来改变、需求本身不清晰、产品决策尚未确定，或我与用户对需求的理解存在 gap。这需要澄清或修改规范，而不只是再加一条 lint rule。
- **一次性偶发错误**：先修复并记录；除非能确定普遍原因，否则无需为每个意外创建 Skill。

## Three Layers for Improving the Factory

| 机制 | 用来防什么 | 最佳载体 | 主要边界 |
| --- | --- | --- | --- |
| 1. Linter / Static Checks | 能被机器明确判定、稳定复现的错误 | Ruff、ESLint、自定义校验、类型检查、pre-commit/CI | 无法代替需求理解或业务语义验证 |
| 2. Agent Skills | 某类任务反复需要特定步骤、上下文、工具或检查流程 | 带 `SKILL.md` 的可复用工作流，附脚本和参考材料 | 需要正确触发、维护和评估；不等于强制执行 |
| 3. `AGENTS.md` / Project Instructions | 所有或大部分开发任务都该遵守的简短全局约定 | 项目级指令（例如工作树路径、检查命令、PR 规范） | 文档是行为指导，不是安全边界；写太多会污染上下文 |

### Choosing the Right Layer

- 如果规则能被确定性程序检查，**优先放在自动化工具 / CI**，而不是只写一句「Agent 不要这样做」。
- 如果解决方法是一组只有特定任务才需要的步骤，把它封装成 **Skill**，利用按需加载减少常驻上下文。
- 如果规则跨任务通用且简短（例如「变更完成前运行哪些命令」），放入 **`AGENTS.md`**。
- 若错误涉及需求 gap，先改善 specification、acceptance criteria 和沟通，而非盲目增加 lint/Skill。
- 这些层可以组合：`AGENTS.md` 指向合适的 Skill；Skill 调用测试脚本；CI 强制校验。职责不要重复一整份长说明。

## Example: Turn a Failure Into a Capability

假设 Agent 修复一个前端表单 bug，但只检查源码、没有在浏览器真正提交表单：

1. **Fix the product**：修好本次 bug，重跑能复现问题的测试。
2. **Identify factory gap**：诊断缺的是验证路径，而不只是「模型不够聪明」。
3. **Add a Skill**：写一个 `verify-web-app` Skill，说明启动方式、测试账号、隔离环境、浏览器交互、证据与清理步骤。
4. **Enforce measurable checks**：稳定场景添加 Playwright Test；CI 跑回归测试。需要的静态规则加入 linter。
5. **Keep global policy small**：`AGENTS.md` 可写「更改用户流程时按验证 Skill 留下可检查证据」，而不是复制整份 Skill。
6. **Evaluate improvement**：用相同初始问题和类似反例对比新旧 harness，观察 bug 复发率、漏检率、token、执行时间与误报。

这与 [[Verification Is All You Need - AI Coding Agent Validation]] 相互补充：**Verification 找出产品问题，Factory Improvement 让系统下次更不容易产出相同问题。**

## Learn From Community Skills

可参考社区的成熟 Skills 设计，不必每次从零开始。

- [pstack](https://github.com/cursor/plugins/tree/main/pstack) 的 [`create-verification-skill`](https://github.com/cursor/plugins/tree/main/pstack/skills/create-verification-skill) 会先调查项目的可运行入口、可交互界面、证据形式与并发隔离要求，再生成面向项目的验证 Skill。
- 借鉴重点不是照抄所有规则，而是研究 **发现任务 → 获取上下文 → 执行 → 留证据 → 处理失败 → 评估** 的设计。
- 引入第三方 Skill 前检查适配性、外部命令、副作用、依赖、权限、维护状态和可能的 prompt injection。对项目做必要改造，再用真实案例验证其收益。
- 别把未经验证的「万能 Skill」当保证：Skill 是流程知识的打包，质量由可观察的结果决定。

## Feedback Loop

```text
Bug or repeated failure
         ↓
Fix the product + make a reproducer
         ↓
Classify cause: deterministic / task-specific / global / requirement gap
         ↓
Linter or CI / Skill / AGENTS.md / clarify spec
         ↓
Rerun known failures + negative cases
         ↓
Measure whether the factory actually improved
```

## My Experience / Current View (2026-10-10)

我希望 Vibe Coding 从「Agent 写错了，我或 Agent 再修」进化成「每次值得学习的错误都反过来改善 harness」。我目前归纳的三个入口是 **linter、Skills、AGENTS.md**；它们服务于不同层级，选择适当载体比不断堆指令更重要。

## Questions / Gaps

- 什么频率的错误值得创建新 Skill？什么时候已有自动化检查就足够？
- 怎样保存小型 regression/evaluation case 库，以评估 harness 修改有没有减少真实重犯？
- 如何防止项目规则膨胀、不同 Skills 互相冲突，或者 Agent 为通过测试而忽略真正的用户目标？

## Related Concepts

- [[Agent Skills]]
- [[Context Engineering for AI Coding Assistants]]
- [[AI-Native Development Workflows]]
- [[Specs Driven Development]]
- [[Agent Hooks]]
- [[Verification Is All You Need - AI Coding Agent Validation]]

# References

- Personal reflection shared in conversation, 2026-10-10.
- [Agent Skills specification](https://agentskills.io/specification)
- [pstack — create-verification-skill](https://github.com/cursor/plugins/tree/main/pstack/skills/create-verification-skill)
- [pstack — adapting the workflow](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/09-make-it-yours.md)
