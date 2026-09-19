# 多代理与任务协调

并行只有在任务可独立、产物边界清楚且合并成本低时才有价值。把强依赖步骤同时交给多个 Agent，通常只会增加冲突。

## 适合并行

- 不同模块的独立调查。
- 实现、测试和文档等边界清晰的子任务。
- 多个方案的独立探索。
- 多仓库但接口已稳定的工作。

## 不适合并行

- 多个任务同时修改同一核心文件。
- 上游决策尚未确定的下游实现。
- 需要共享临时状态或频繁同步的工作。

## 协调方法

1. 主任务定义共同目标、接口和验收标准。
2. 每个 Agent 获得一个可独立完成的子任务。
3. 使用 worktree 或独立分支隔离写入。
4. 用状态等待而不是高频轮询。
5. 汇总时重新运行全量验证并人工审查冲突。

## 来源

- [最新动态 — OpenAI Docs](https://developers.openai.com/zh-Hans/docs/whats-new)，访问于 2026-09-19。
- [Run long-horizon tasks with Codex](https://developers.openai.com/es-419/blog/run-long-horizon-tasks-with-codex)，访问于 2026-09-19。
