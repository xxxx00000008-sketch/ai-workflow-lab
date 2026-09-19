# Skills 与 Plugins

## 区别

- **Skill**：针对某类任务的可复用说明、资料和脚本。
- **Plugin**：可安装分发包，可组合 Skills、MCP 服务器、Hooks 和元数据。
- **MCP**：向 Agent 提供外部系统的数据和操作工具。

## 成熟路径

1. 先在真实任务中跑通工作流。
2. 重复成功后整理为 Skill。
3. 用代表性输入、失败场景和权限边界测试。
4. 需要团队分发或外部工具时再包装为 Plugin。

不要为一次性要求创建 Skill；不要只为“看起来完整”创建 Plugin；不要把连接器授权误认为插件安装自带权限。

安装前审查来源和内容。MCP 与 Hooks 可能读取数据或执行动作，应遵循最小权限；外部写入、发布和删除必须有明确授权。

## 来源

- [插件 — ChatGPT Learn](https://learn.chatgpt.com/zh-Hans/docs/plugins)，访问于 2026-09-19。
- [构建插件](https://learn.chatgpt.com/zh-Hans/docs/build-plugins)，访问于 2026-09-19。
