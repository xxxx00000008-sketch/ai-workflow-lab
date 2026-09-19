# Roadmap

本路线图描述可交付成果，而不是承诺固定发布日期。工具变化很快，结论应以可复现的实验和标注日期的资料为依据。

## V0.1 — ChatGPT & Codex Foundation（文档基线完成）

目标：建立统一研究方法，并完成 ChatGPT 与 Codex 桌面端核心能力的第一轮系统梳理。

### 研究顺序

- [x] Projects
- [x] Memory
- [x] Library
- [x] Search
- [x] Work
- [x] Browser
- [x] Plugins
- [x] Scheduled Tasks
- [x] Sites
- [x] Codex 桌面端功能地图
- [x] Projects 与 Threads
- [x] 本地、云端与 worktree
- [x] 文件、终端、浏览器与产物预览
- [x] Git、Diff 与 Review
- [x] AGENTS.md
- [x] Skills、Plugins 与 MCP
- [x] Automations
- [x] 多代理与任务协调
- [x] 权限与安全
- [x] Codex 桌面端从零到交付主教程
- [x] ChatGPT Project + Codex Project 实际工作提效手册

### 第一轮文档完成标准

- 至少一份工具知识卡，说明能力、适用场景、限制和风险
- 能明确区分官方说明、当前环境观察和待验证内容
- 整体至少包含可复现实验、可复用工作流和真实案例
- 重要结论能追溯到官方资料或实验记录
- 内容经过自查，明确标注测试日期与可能失效的部分

### 阶段验收

- 仓库结构、贡献规范以及 Issue/PR 模板可用
- ChatGPT 与 Codex 主题均达到上述第一轮标准
- 形成 Chat、Work 与 Codex 能力地图和选择指南
- 完成内部链接、格式和 Git 状态校验

### 持续验证清单

- [ ] 普通 Chat、默认 memory Project 与 project-only memory Project 对照
- [ ] 不同订阅方案的 Library、文件和工具限制
- [ ] Browser 登录、表单、审批与提示注入测试
- [ ] 共享 Project 和插件的角色权限测试
- [ ] Codex 云端环境与本地环境一致性测试
- [ ] 多 worktree 并行开发与合并冲突测试
- [ ] Automation 失败、静默通知和恢复测试
- [ ] Sites 保存、生产部署、权限与持久化数据测试
- [ ] 发布 `v0.1.0` 版本及变更说明

## 后续方向

- **V0.2 — Commercial Workflows**：用 Codex 完成开源运营、内容生产和独立开发的可销售工作流
- **V0.3 — Claude Code Deep Dive**：记忆、代理、Hooks、MCP 与工程工作流
- **V0.4 — Cross-tool Workflows**：跨工具协作、知识管理、内容创作与独立开发案例

具体版本范围会根据前一阶段的实验结果调整。
