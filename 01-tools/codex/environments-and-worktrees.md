# 本地、云端与 Worktree

## 本地环境

适合需要现有未提交修改、本机工具、私有依赖或即时交互的任务。直接在当前 checkout 工作时，Agent 的修改会影响同一目录，应先确认工作区状态。

## Worktree

worktree 是同一 Git 仓库的隔离工作目录。它适合并行功能、实验性重构和长任务，可以减少多个 Agent 互相覆盖文件或分支的风险。

## 云端环境

适合不依赖当前电脑、需要后台继续运行的任务。云端有独立环境、权限和仓库访问配置，不能假设与本机完全一致。

## 选择表

| 场景 | 建议 |
| --- | --- |
| 只读调查当前代码 | 当前 checkout |
| 小而明确、必须包含未提交修改 | 当前 checkout，先审查状态 |
| 并行功能或不确定实验 | worktree |
| 电脑关闭后仍需继续 | 支持时使用云端 |

启动前确认仓库、基础分支、未提交修改、依赖准备、验证命令和允许访问的目录。

## 来源

- [最新动态 — OpenAI Docs](https://developers.openai.com/zh-Hans/docs/whats-new)，访问于 2026-09-19。
- [Run long-horizon tasks with Codex](https://developers.openai.com/es-419/blog/run-long-horizon-tasks-with-codex)，访问于 2026-09-19。
