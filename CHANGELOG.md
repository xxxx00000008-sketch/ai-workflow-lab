# Changelog

本项目的重要变更记录在此文件中，格式参考 Keep a Changelog，版本遵循语义化版本。

## [Unreleased]

### Added

- 增加并升级 `tokens-store` Token 聚合平台案例为 FDE 终态方案，覆盖产品契约、领域治理、风险分级、Agent 工程控制面、质量门禁、渐进发布、生产可观测与反馈闭环
- 增加项目章程，明确使命、目标用户、双轴内容模型、五级产物、案例验收标准和商业边界
- 增加真实案例模板，并补充工作流的状态、执行证据和效率对比字段
- 建立 AI 工具、原理、工作流、案例、对比、提示词、实验与模板八类内容结构
- 定义 V0.1 ChatGPT Deep Dive 路线与完成标准
- 增加贡献规范、行为准则以及 Issue/PR 模板
- 创建 ChatGPT Projects 首个研究入口
- 完成 ChatGPT Projects 第一轮研究闭环：知识卡、真实交接实验、生命周期工作流与案例
- 补齐 ChatGPT Memory、Library、Search、Work、Browser、Plugins、Scheduled 与 Codex 切换指南
- 建立 Codex 桌面端功能地图及 Projects、worktree、工具、Git Review、AGENTS.md、Skills、Automations、多代理和权限专题
- 增加 Chat、Work 与 Codex 选择指南，以及 Codex 桌面交付工作流与实战实验
- 增加面向新用户的 Codex 桌面端从零到可审查交付主教程
- 补充 ChatGPT Sites 的创建、托管、部署、权限和 Codex 协作边界
- 增加 ChatGPT + Codex 实际工作提效手册，比较传统、AI 协作和自动化流程
- 增加基于本仓库真实提交记录的开源项目周维护案例

### Changed

- 明确 FDE 案例中 ChatGPT Project 与 Codex Project 的职责、目录归属、非自动同步边界，以及交付契约从产品讨论进入 Git 并回流生产结果的完整衔接
- 将 FDE 终态方案调整为“业界工程标准为骨架、OpenAI 官方产品能力为实现映射、项目约定为落地细则”，并接入 DORA、NIST SSDF、OWASP ASVS、SLSA、Google SRE、OpenTelemetry 和 OpenSSF 基线
- 产品能力文档新增“入门用法 + 高阶用法”硬性结构，并在 ChatGPT Projects 与 Codex 主教程中先行应用
- 将真实案例交付形式调整为单个 Markdown：写清自动化步骤并展示建议目录树，不再要求创建目录树中的示例文件
- 将路线图从功能覆盖调整为“能力基线 + 真实工作流验证”，避免把文档完成等同于行为实测
- 将 ChatGPT Project + Codex Project 明确为长期知识库场景的一种组合，而非项目唯一主线
- 收紧工作流、案例和贡献内容的证据及可复现要求
