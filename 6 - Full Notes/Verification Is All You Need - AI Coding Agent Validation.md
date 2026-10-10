# Verification Is All You Need - AI Coding Agent Validation

Tags: [[3 - Tags/agentic ai|agentic ai]] [[3 - Tags/mcp|mcp]]

2026-10-10 07:53

## Core Idea

**Verification is all you need** 是我近期对 Vibe Coding 的一个重要判断：AI Agent 不仅要能写代码，也要能像用户一样操作产物、观察结果、提供证据，形成 **implement → run → observe → assert → fix → rerun** 的闭环。浏览器就是 Agent 的「手和眼睛」，让它不只检查源码，还能检查真实 UI 行为。

这里的 `all you need` 是强调验证的杠杆作用，不代表需求澄清、架构设计、代码审查、安全检查或单元测试可以省略。我的目标是减少「Agent 说完成，但实际无法使用」所造成的返工，而不是追求无人监督地点击页面。

## Three Browser Interaction Forms

| 形态 | 代表工具 | Agent 如何观察与操作 | 适用情境及代价 |
| --- | --- | --- | --- |
| MCP 工具接口 | Playwright MCP | Agent 通过有类型的工具调用获取页面结构、点击、输入和截图 | 能与非编码型 Agent 集成，工具调用直观；暴露的工具 schema 和返回内容可能消耗上下文 |
| CLI + Skills | [Playwright CLI](https://github.com/microsoft/playwright-cli)、[agent-browser](https://github.com/vercel-labs/agent-browser) | Coding Agent 从终端运行短命令，读取精简 accessibility/DOM snapshot，并继续操作 | 适合已有 shell/编码能力的 Agent；通常更易控制 token 用量，也容易纳入现有开发命令 |
| Visual / Computer Use | 截图驱动的 Computer Use、[Browser Use](https://github.com/browser-use/browser-use) 的视觉模式 | 模型从截图定位控件，可用坐标等方式交互 | 适合 canvas、复杂控件或语义树不可靠的界面；图像推理可能较慢、较贵、较脆弱 |

### Important Corrections to My Mental Model

- **MCP 不是「已经过时」**：对于 Coding Agent，CLI 通常更精简，但 MCP 仍可用于丰富的交互式探索。上下文负担取决于工具数量、服务端设计、客户端是否延迟加载工具，以及返回内容是否裁剪；不是所有 MCP 都会把所有定义和页面一次性塞进上下文。
- **CLI 不等于自动生成持久测试**：CLI 能驱动浏览器；要把一次探索转成可靠、可回放的 Playwright Test，仍需编写或生成 `.spec.ts`、增加断言并维护稳定 locator。官方 [Playwright codegen](https://playwright.dev/docs/codegen) 可以协助录制测试，但录制后也要审阅。
- **Browser Use 不只是纯图片识别**：它可组合页面元素信息、DOM 和截图，视觉可配置；「Computer Use」也不是一套与 CLI/MCP 完全互斥的技术栈。
- **视觉操作不是反自动化机制的万能绕过**：网站仍可能识别自动化或限制访问。仅应在获授权的环境中使用，不把截图点击当成规避风控的保证。

## A Practical Verification Loop

1. **Define the proof**：先写用户行为和可观察的完成条件。例如「登录后新增项目，列表出现对应项目；刷新后仍存在」。
2. **Start a controlled app**：在本地或允许测试的 UAT 启动应用，准备测试账号和隔离数据。不要让多个 Agent 争抢同一浏览器会话。
3. **Drive the UI**：优先用 CLI + 可访问性定位完成真实用户路径；必要时截图判断布局与视觉结果。
4. **Assert, do not merely click**：验证 URL、关键文字、表单反馈、业务状态以及持久化结果；检查控制台/网络错误。失败时留下日志、截图或 trace。
5. **Make it repeatable**：把稳定的高价值路径变成 Playwright Test（E2E/回归），纳入 CI；临时探索命令不应冒充测试资产。
6. **Repair and rerun**：Agent 根据失败证据定位问题，修复后重新跑原始路径以及关联回归测试；需要时再由人审查。

```text
Specify acceptance criteria
       ↓
Agent implements feature
       ↓
CLI / MCP / Visual browser exploration
       ↓
Check UI, state and evidence ── failure ──> diagnose → fix
       ↓                                      │
Promote repeatable paths to Playwright Test ←─┘
       ↓
Review + CI evidence + human approval
```

## Practical Choices and Trade-offs

- **默认选择**：对有终端访问能力的 Codex 类 Coding Agent，先试 CLI + Skill，输出精简 snapshot；需要持久回归时使用 Playwright Test。
- **MCP 仍有价值**：Agent 只能走工具接口，或者需要丰富页面检查能力时，保留 Playwright MCP；用实际 token/时延衡量成本。
- **视觉作为补充**：DOM 无法表达完整状态、交互基于画布或视觉检查至关重要时采用截图驱动，但确认截图中的结果，而不是默认点击成功。
- **验证有边界**：模拟用户可以减少 UI 返工，无法单独证明并发安全、权限边界、后台逻辑正确性或生产环境可用性。需要单元、集成、API、E2E 等分层测试。

## My Experience / Current View (2026-10-10)

近期我在比较 Playwright MCP、Playwright CLI、agent-browser 与 Browser Use，最在意的是让 Vibe Coding Agent **自主验证功能、减少返工，同时控制上下文/token 消耗**。这是我目前正在探索的技术选择，并非已经实测证明任何工具永远胜出。

## Questions / Gaps

- 对同一条 UAT 场景，CLI、MCP、视觉模式的成功率、token、耗时和失败恢复成本分别是多少？
- 如何在需要本人先登录的环境安全复用授权会话，并避免泄露 cookie、storage state？
- 哪些探索性浏览步骤值得提升为稳定的回归测试？如何防止录制脚本变成脆弱的 UI 测试？

## Related Concepts

- [[AI-Native Development Workflows]]
- [[Agent Skills]]
- [[Headless Coding Agent Automation]]
- [[Coding Agent Permissions and Sandboxing]]
- [[Fix the Factory, Not Just the Product - Improving the Coding Agent Harness]]

# References

- Personal reflection shared in conversation, 2026-10-10.
- [Playwright CLI — CLI versus MCP](https://github.com/microsoft/playwright-cli)
- [Playwright Test generator](https://playwright.dev/docs/codegen)
- [agent-browser](https://github.com/vercel-labs/agent-browser)
- [Browser Use](https://github.com/browser-use/browser-use)
- [pstack create-verification-skill](https://github.com/cursor/plugins/tree/main/pstack/skills/create-verification-skill)
