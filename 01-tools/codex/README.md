# Codex 桌面端

> 状态：第一轮完成
>
> 最近验证：2026-09-19
>
> 环境：Windows ChatGPT 桌面应用中的 Codex、本地 Git 仓库

Codex 是面向代理式工作的桌面工作台：把项目文件、终端、Git、网页、插件、Skills、自动化和多任务协作组织在一个可监督环境中。它的价值不是“一次生成更多代码”，而是让 Agent 能读取真实状态、执行工具、验证结果并交付可审查变更。

## 功能地图

| 能力 | 解决的问题 | 指南 |
| --- | --- | --- |
| Projects 与 Threads | 如何组织长期项目和单次成果 | [`projects-and-threads.md`](projects-and-threads.md) |
| 本地、云端与 worktree | 工作在哪里运行，如何隔离变更 | [`environments-and-worktrees.md`](environments-and-worktrees.md) |
| 文件、终端、浏览器与预览 | Agent 如何观察和操作真实环境 | [`tools-and-artifacts.md`](tools-and-artifacts.md) |
| Git、Diff 与 Review | 如何检查、评论和交付变更 | [`git-and-review.md`](git-and-review.md) |
| `AGENTS.md` | 如何保存项目级长期规则 | [`agents-md.md`](agents-md.md) |
| Skills 与 Plugins | 如何复用和分发工作流 | [`skills-and-plugins.md`](skills-and-plugins.md) |
| Automations | 如何委托周期性后台工作 | [`automations.md`](automations.md) |
| 多代理与任务协调 | 如何并行且避免互相覆盖 | [`multi-agent.md`](multi-agent.md) |
| 权限与安全 | 如何控制文件、网络和外部动作 | [`permissions-and-safety.md`](permissions-and-safety.md) |

## 最小闭环

1. 选择正确项目目录和执行环境。
2. 为一个可验收结果开启独立 Thread。
3. 让 Codex 先读取 `AGENTS.md` 和相关文件。
4. 实施修改并运行测试或其他验证。
5. 在 Diff/Review 中检查变更。
6. 提交到独立分支或形成明确交付物。
7. 将新规则和结论沉淀回仓库。

## 官方定位

OpenAI Docs 将当前桌面端描述为支持并行项目聊天、内置 Git 审查、worktree、Skills、计划任务和语音输入的工作空间。这些能力现已整合到 ChatGPT 桌面应用中的 Codex。[来源](https://developers.openai.com/zh-Hans/docs/whats-new)

## 与其他入口的关系

- **Chat**：快速问答和轻量讨论。
- **Work**：面向知识工作的多步骤交付物。
- **Codex**：需要本地工程上下文、代码、终端、Git、验证和长任务监督。
- **CLI**：终端优先、脚本化和无界面运行。
- **IDE extension**：围绕当前编辑器文件的紧密协作。

选择指南见 [`Chat、Work 与 Codex`](../../05-comparisons/chat-work-codex.md)。

## 来源范围

本文组优先使用已打开的 OpenAI Docs / ChatGPT Learn 页面和当前环境观察。官方 Codex 手册抓取端点在 2026-09-19 返回 HTTP 403，因此没有把未读取的手册内容当作证据。
