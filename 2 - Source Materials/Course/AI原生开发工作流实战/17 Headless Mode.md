2026-04-12 12:16

# 17 Headless Mode
- Headless mode switch CC from interactive app into an executable API
- It allow CC to be integrated into script or pipeline as a programmable function
- CC execute and print the output to stdout

![[Attachments/Pasted image 20260412121929.png]]

## Standard IO
```bash
# 场景：快速根据获取一个Git Commit Message建议
claude -p "Stage我的修改，然后生成一条符合Conventional Commit规范的Message" --allowedTools "Bash,Read" --permission-mode acceptEdits

# 使用cat将文件内容通过管道传递给claude
cat nginx-error.log | claude -p "请分析这份Nginx错误日志，总结出最主要的错误类型和可能的原因。"
```

## Structured Output
```bash
claude -p "使用go-code-security-reviewer subagent 审查@internal/converter/converter.go，检查是否有安全漏洞" --output-format json > review_report.json
```

# References
