2026-10-10 07:53

Tags: [[3 - Tags/git|git]] [[3 - Tags/agentic ai|agentic ai]]

# Git Worktrees for Parallel AI Development

## Core Idea

**Git worktree 让同一个 Git repository 同时有多个独立的工作目录**，每个工作目录可以检出不同的 branch 或 commit。它是我给 AI Coding Agent 做平行开发、隔离功能开发与紧急 hotfix 的重要工具。

我最直观的类比是「多一个 repository 文件夹」。但准确来说，worktree **不是完整的 `git clone`**：不同 worktree 各自拥有工作文件、索引和 HEAD 等工作状态，同时共享底层 repository 的大部分 Git 对象和 refs，因此通常比另起一份 clone 更轻量。

## Branch vs Worktree vs Clone

| 机制 | 隔离了什么 | 典型体验 |
| --- | --- | --- |
| Git branch | 提交历史的不同引用；只建 branch 不产生第二份工作目录 | 在同一目录 checkout 时要切换文件版本，可能碰到未提交改动 |
| Git worktree | 不同工作目录与其工作状态；共享同一仓库的 Git 存储 | 两个目录并行打开、编辑、测试，无需来回 stash |
| Git clone | 独立 repository 与工作目录 | 隔离程度更高，但需要独立拉取和维护 Git 对象/仓库设置 |

一个 branch 通常不能同时在两个 linked worktrees 中 checkout；创建平行任务时应使用各自独立的 branch。

## My Primary Use Case: Feature + Production Hotfix

我正在原目录开发新功能，但生产环境突然需要 hotfix。我不想把尚未完成的新功能和修复代码混在一起：

1. 保留正在使用的 feature worktree，不必为了 hotfix 强行 stash 或提交 WIP。
2. 从最新可用的生产基线建立 **新 branch + 新 worktree**。
3. 把 hotfix 交给另一个 Codex 会话或自己在新目录处理，独立运行相关测试。
4. 在 Fork 中分别检查两个工作树的 diff 与 commit；hotfix 经 review 后走 PR/部署流程。
5. 原功能仍留在旧目录，不需要切换 checkout；合适时同步 hotfix 进 feature branch。

## A Predictable Directory Convention

我希望通过项目 `AGENTS.md` 固定 worktree 位置和命名，避免 Coding Agent 随机在根目录或仓库内部散落文件夹。

```text
workspace/
├── my-repo/                        # main checkout (feature development)
└── worktrees/
    └── my-repo/
        ├── hotfix-login/
        ├── feature-reporting/
        └── fix-validation/
```

- 固定 root：`../worktrees/<repo-name>/<task-name>/`（示意路径，应按项目所在位置确认）。
- task-name 用可读的 kebab-case；Git branch 则可用 `hotfix/login`、`feature/reporting` 这样的命名。
- worktree 尽量放在仓库根目录之外，避免意外进入 Git 追踪范围。
- **这是一条适合写进实际代码项目 `AGENTS.md` 的管理约定**，不是 Git 的强制默认行为。此笔记只记录规则，不自动修改知识库本身的管理文件。

### Example Commands

以下示例假设主工作目录为 `workspace/my-repo/`，并且远端主分支是 `origin/main`：

```bash
# 在主仓库目录执行
git fetch origin
git worktree add -b hotfix/login ../worktrees/my-repo/hotfix-login origin/main

# 查看所有 worktrees
git worktree list

# 在新工作目录单独提交、推送、创建 PR
cd ../worktrees/my-repo/hotfix-login

# 完成、确认不再需要且工作目录没有未保存修改后，
# 回到主仓库执行：
git worktree remove ../worktrees/my-repo/hotfix-login
git branch -d hotfix/login  # 仅在该分支已安全合并且不再需要时
```

注意：示例的 `cd` 是从 `workspace/my-repo/` 出发；清理命令中的路径也以回到主仓库为前提。不要直接删除 worktree 文件夹替代 `git worktree remove`。

## Managing Worktrees With Fork

我日常使用 Git GUI 工具 **Fork** 来观察多个工作树与分支：

- 查看已有 worktrees、分别打开工作目录/标签页并检查各自的未提交变更。
- 同时对照 feature branch 与 hotfix branch，Review AI 在不同目录修改的内容。
- Fork 是这里的 Git 客户端产品名称，**不是** GitHub 的「fork repository」机制。
- Fork 提供 worktree 管理和标签页功能，但实际操作仍要注意对应路径和 branch，不能因为 GUI 显示方便就忽略 Git 状态。

## Isolation Boundaries and Pitfalls

- **代码目录隔离不等于运行环境隔离**：两个 worktrees 可能仍共用端口、数据库、缓存、`.env`、认证状态或测试数据；并行跑服务时需明确区分。
- **共用 Git 仓库存储**：fetch、branch refs、某些 Git 配置等变化会影响其他 worktrees。不要假设完全独立。
- **Agent 必须锁定工作目录**：每个 Codex 会话指定对应 worktree，提交前检查 `pwd`、`git status`、当前 branch 与 diff，避免误写别人的任务。
- **清理前检查状态**：不要对有未保存变更的工作目录强制移除；不要因为 PR 合并了就删除仍需要的测试资料或本地配置。
- **不是每个任务都要创建 worktree**：只有并行开发、hotfix、独立 review、不同方案试验能明显节省上下文切换时才值得用。

## My Experience / Current View (2026-10-10)

我希望 worktrees 是一种 **可管理、可观察的并行开发单元**：统一 `worktrees/<repo>/<task>` 命名，在 Fork 中检查 Agent 的实际改动，避免功能开发和生产修复互相污染。这个约定来自我的个人使用习惯，而不是 Git 内建规定。

## Related Concepts

- [[AI-Native Development Workflows]]
- [[Coding Agent Permissions and Sandboxing]]
- [[Agent Teams and Orchestration]]
- [[Context Engineering for AI Coding Assistants]]
- [[Verification Is All You Need - AI Coding Agent Validation]]

# References

- Personal reflection shared in conversation, 2026-10-10.
- [Git worktree — official documentation](https://git-scm.com/docs/git-worktree.html)
- [Fork for Windows — worktree support release notes](https://fork.dev/releasenoteswin)
- [Fork — worktree management updates](https://fork.dev/releasenotes)
