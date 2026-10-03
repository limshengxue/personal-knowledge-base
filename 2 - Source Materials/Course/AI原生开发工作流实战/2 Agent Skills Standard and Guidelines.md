2026-04-11 12:21

# 2 Agent Skills Standard and Techniques
## Structure of Agent Skills
- The core is `SKILL.md` require certain standard field
	- `name` - 1 to 64 chars, only lowercase, number and `-`
	- `compatibility` -  to declare dependencies required by the Skill. Eg. `Need Python 3.11+`
	- `license` and `metadata`
- Follow `Progressive Disclosure` principle, the `SKILL.md` shall not go over 500 lines
![[Attachments/Pasted image 20260411122430.png]]

## Techniques for Effective Skill
### Need Empathy
- Explain *the WHY* instead of emphasizing *MUST* and *NEVER*

```
✅ 具有“共情能力”的写法：“在处理数据库连接时，请采用单例模式。因为我们的云函数环境并发量很大，频繁创建连接会导致下游数据库连接池耗尽，曾引发过严重的线上故障。为了方便团队审查，请在输出分析时，保持结构清晰，优先使用标题和要点列表。”
```

### Focus on `description`
- Must write like a *salesman* as current LLM tends to under-trigged
```
❌ 软弱的描述：“提取 PDF 文本并格式化数据。” ✅ 高触发率的描述：“提取 PDF 文本，识别段落结构，并能精准提取表格数据。请务必在用户上传了 PDF 文件、提到 PDF 解析、表单填写，或者要求从文档中提取结构化数据时，立即且优先使用本技能，哪怕用户没有直接说出‘PDF 提取’几个字！”
```

### Remove Certainty, Embrace Scripting
- If precise execution is required, leverage `/scripts`
```
“当提取出关键数据后，请不要自行格式化。请直接调用 Bash(python scripts/formatter.py data.json)。脚本会返回最终的格式化结果。”
```

### Evaluation
- Skill required Evals and Assertions

## AI Generated Skill
- There has been a concept known as `Meta-skills` that help agent to define skill according to user requirement
- Eg. `skill-creator` created by Anthropic
- Main Agent capture requirement, write and test the Skill
- Grader Agent evaluate the requirement
- Comparator and Analyzer do double-blind test to optimize the Skill

# References
