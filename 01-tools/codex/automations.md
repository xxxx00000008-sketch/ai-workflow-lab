# Automations

Automation 把指令、可选 Skills 和运行时间组合起来，用于周期性检查、整理、报告和监控。结果进入可审查队列，而不是默认直接执行所有后续动作。

## 高价值场景

- 每日 Issue 分诊。
- 汇总 CI 失败和回归风险。
- 生成发布简报。
- 检查过期文档、失效链接或依赖更新。
- 监控任务完成、失败或需要人工输入的状态。

## 设计规则

1. 任务应可重复、输入稳定、完成标准清楚。
2. 默认只读，写操作单独授权。
3. 无变化时保持安静，只在重大变化、失败或需要行动时通知。
4. 保留运行记录和失败处理。
5. 本地自动化依赖主机可用；需要持续运行时选择支持的云端环境。

ChatGPT Scheduled 偏向提醒和知识工作；Codex Automation 更适合项目、仓库、Skills 和工程工具链。

## 来源

- [最新动态 — OpenAI Docs](https://developers.openai.com/zh-Hans/docs/whats-new)，访问于 2026-09-19。
- [开始使用 ChatGPT Work](https://learn.chatgpt.com/zh-Hans/docs/get-started-with-work)，访问于 2026-09-19。
