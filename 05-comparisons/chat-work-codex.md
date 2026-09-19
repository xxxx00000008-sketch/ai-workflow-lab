# Chat、Work 与 Codex 选择指南

> 最近验证：2026-09-19

| 维度 | Chat | Work | Codex |
| --- | --- | --- | --- |
| 主要目标 | 获得答案、解释或短草稿 | 完成多步骤知识工作成果 | 在真实工程环境中实施并验证 |
| 典型输入 | 文本、少量附件 | 多来源、文件、插件和工具 | 仓库、文件、终端、Git、插件 |
| 典型输出 | 对话答案 | 报告、表格、演示、流程 | 代码、文档、测试、Diff、提交 |
| 执行时间 | 短 | 中到长 | 中到长，可并行 |
| 状态载体 | 当前对话 | Project、来源和产物 | 仓库、Thread、worktree 和 Git |
| 人工重点 | 判断答案 | 审查产物与外部动作 | 审查 Diff、测试、安全和合并 |

## 决策规则

1. 只需要“告诉我”：Chat。
2. 需要“替我完成一个可审查成果”：Work。
3. 需要“进入项目、修改并证明它有效”：Codex。
4. 需要长期共享背景：把上述入口放进 Project。
5. 需要周期重复：增加 Scheduled 或 Automation。

一个任务可以逐步升级：先在 Chat 澄清问题，再由 Work 整理方案，最后由 Codex 在仓库中实现。不要一开始就使用最重的执行面。

## 来源

- [开始使用 ChatGPT Work](https://learn.chatgpt.com/zh-Hans/docs/get-started-with-work)，访问于 2026-09-19。
- [最新动态 — OpenAI Docs](https://developers.openai.com/zh-Hans/docs/whats-new)，访问于 2026-09-19。
