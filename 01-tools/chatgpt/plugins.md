# ChatGPT Plugins

> 最近验证：2026-09-19

## 定位

插件是可安装的能力包，可以包含 Skills、MCP 工具和生命周期 Hooks。ChatGPT 与 Codex 使用统一插件目录，但各界面的支持范围可能不同。

## 选择层级

| 需求 | 使用 |
| --- | --- |
| 重复执行同一套方法 | Skill |
| 访问 Slack、Drive、GitHub 等外部系统 | MCP/连接器 |
| 分享包含工作流和工具的稳定能力 | Plugin |
| 强制执行生命周期检查 | Hook |

## 使用流程

1. 审查插件来源、权限和数据去向。
2. 安装后完成必要的服务授权。
3. 在新对话用代表性只读任务测试。
4. 明确使用 `@插件名`，或让系统按任务选择。
5. 写操作从最小权限开始，并保留审批。

安装插件不等于自动获得外部服务权限；MCP 服务仍有独立身份验证，主机沙盒也继续生效。

## 来源

- [插件 — ChatGPT Learn](https://learn.chatgpt.com/zh-Hans/docs/plugins)，访问于 2026-09-19。
- [技能控制](https://learn.chatgpt.com/zh-Hans/docs/enterprise/skills)，访问于 2026-09-19。
