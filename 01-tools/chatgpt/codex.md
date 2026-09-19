# 从 ChatGPT 切换到 Codex

> 最近验证：2026-09-19

当任务从“给我答案”变成“在本地项目里完成并验证变更”时，切换到 Codex。

## 切换信号

- 需要读取或修改多个本地文件。
- 需要运行测试、构建、终端命令或 Git。
- 需要在独立 worktree 中并行探索。
- 需要审查差异、评论具体代码行或提交变更。
- 任务可能持续较长时间，需要中途指导。

## 不必切换

纯解释、短文改写、开放式讨论和不涉及本地状态的一次性问题，优先使用 Chat。

## 交接格式

向 Codex 提供：目标、仓库、相关文件、约束、完成标准、验证命令、允许的外部操作。长期规则放入 `AGENTS.md`，不要每次重复粘贴。

继续阅读 [`Codex 桌面端`](../codex/README.md)。

## 来源

- [最新动态 — ChatGPT Learn](https://developers.openai.com/zh-Hans/docs/whats-new)，访问于 2026-09-19。
- [开始使用 ChatGPT Work](https://learn.chatgpt.com/zh-Hans/docs/get-started-with-work)，访问于 2026-09-19。
