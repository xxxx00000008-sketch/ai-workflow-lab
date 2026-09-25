# AI Workflow Lab

面向独立开发者、FDE 和小型技术团队的中文实证知识库，帮助已经使用 AI 的人从零散对话升级为可复现、可审查、可自动化的工作系统。

这里不是产品按钮说明书或提示词合集。项目通过官方资料、真实任务和对照实验，验证 AI 工作流能否在时间、质量、可复现性、成本和风险方面优于原有方式，并把有效方法沉淀为工作流、案例、模板、Skills 与自动化资产。

> 当前阶段：**V0.1 — ChatGPT & Codex 能力基线完成；V0.2 — 三条真实工作线验证进行中**

## 要解决的核心问题

目标用户通常已经会使用 AI 聊天，但仍存在一条完整断链：

```text
不知道产品能力
→ 不知道什么场景该用
→ 组合不出稳定流程
→ 缺少质量与风险控制
→ 无法证明比原方法更有效
```

## 当前聚焦

V0.2 只优先验证三条工作线：

1. **AI 原生产品与软件交付**：需求、研究、实现、测试、Review、发布、监控和反馈闭环。
2. **研究、决策与知识生产**：来源核验、分析、写作、审校和知识沉淀。
3. **个人与小团队自动化**：项目跟踪、周期检查、报告和异常通知。

会议、表格、演示、内容生产和开源运营暂时作为上述工作线中的具体任务，不再作为同等级扩张方向。完整定位与验收标准见 [`project-charter.md`](project-charter.md)。

## 第一个端到端验证案例

首个旗舰案例是 [`差价 AI：从中国市场 Demo 到全球产品的 FDE 交付验证`](04-cases/chajia-ai-end-to-end-validation.md)。案例以 [OpenTheRank](https://opentherank.com/zh/) 为参考产品，验证从用户问题、价格来源、数据标准、AI 协作、工程交付、发布检查到持续更新和市场反馈的完整流程。

当前 [`差价 AI Demo`](https://chajia-ai.ixm009.chatgpt.site/)只是交付载体之一；目标不是“做出一个网站”，而是先为中国用户验证可信、可维护的 AI 订阅比价服务，再逐步扩展至全球市场。

现有 [`tokens-store` FDE 方案](04-cases/tokens-store-codex-development-workflow.md)继续作为目标架构参考，不再承担第一个端到端验证案例的角色。

## 按目标开始

| 你的目标 | 建议入口 |
| --- | --- |
| 了解 ChatGPT 当前能力 | [`ChatGPT 功能地图`](01-tools/chatgpt/) |
| 用 Codex 完成本地工程任务 | [`Codex 桌面端地图`](01-tools/codex/) |
| 判断使用 Chat、Work 还是 Codex | [`选择指南`](05-comparisons/chat-work-codex.md) |
| 从单次使用升级为工作流 | [`ChatGPT + Codex 提效手册`](03-workflows/chatgpt-codex-productivity-playbook.md) |
| 查看真实产品的端到端验证 | [`差价 AI 案例`](04-cases/chajia-ai-end-to-end-validation.md) |
| 复用模板开展自己的实验 | [`Templates`](08-templates/) |

## 内容结构

八个目录是内容存储结构，不是用户必须按顺序阅读的课程：

| 模块 | 回答的问题 | 目录 |
| --- | --- | --- |
| Tools | 工具有什么能力，如何使用？ | [`01-tools`](01-tools/) |
| Principles | 为什么有效，有什么边界？ | [`02-principles`](02-principles/) |
| Workflows | 多个能力如何组合成流程？ | [`03-workflows`](03-workflows/) |
| Cases | 如何在真实任务中落地？ | [`04-cases`](04-cases/) |
| Comparisons | 同类工具如何选择？ | [`05-comparisons`](05-comparisons/) |
| Prompts | 哪些指令值得复用？ | [`06-prompts`](06-prompts/) |
| Experiments | 结论由什么实验支持？ | [`07-experiments`](07-experiments/) |
| Templates | 如何低成本重复研究？ | [`08-templates`](08-templates/) |

## 研究与交付闭环

`官方资料 → 能力卡 → 可复现实验 → 工作流 → 真实案例 → 模板 / Skill / Automation`

每项工作尽可能比较：

`传统手工 → 单次 AI 协作 → 标准化流程 → 半自动化 → 受控自动化`

项目使用独立的证据等级和自动化成熟度，不再把文档类型等同于验证程度。当前优先级见 [`ROADMAP.md`](ROADMAP.md)。

**ChatGPT Project 管理研究和决策上下文、Codex Project 管理本地文件与工程交付**是一种常用组合模式，不是本项目的唯一架构。

网站只是发布或交付渠道。项目真正验证的是背后的数据、流程、质量控制和工作结果。

## 参与贡献

欢迎通过 Issue 提交问题、实验想法或勘误。涉及内容变更时，请先阅读 [`CONTRIBUTING.md`](CONTRIBUTING.md)，并尽量使用仓库内的模板保留可复核证据。

## 许可

本项目采用 [MIT License](LICENSE)。引用第三方资料时，仍需遵守原作者及来源的许可条款。
