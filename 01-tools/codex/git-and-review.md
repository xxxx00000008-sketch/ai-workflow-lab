# Git、Diff 与 Review

Codex 的结果应以真实差异和验证结果为准，而不是以“已完成”的口头声明为准。

## 标准流程

1. 开始前检查分支和工作区是否干净。
2. 对较大任务创建独立分支或 worktree。
3. 修改过程中保持范围聚焦，不覆盖无关用户改动。
4. 运行测试、lint、构建或内容校验。
5. 查看 Diff，检查意外删除、敏感信息和范围漂移。
6. 对具体行添加 Review 评论并要求修订。
7. 以清晰提交信息保存变更，推送后进入 PR 审查。

Codex Review 是额外审查者，不替代测试、分支保护、必需审批和领域专家。可把重复出现的仓库特定问题写进 `AGENTS.md` 的 Review Rules。

## 来源

- [Custom code review rules for Codex](https://developers.openai.com/de-DE/blog/custom-code-review-rules-for-codex)，访问于 2026-09-19。
- [AGENTS.md instructions](https://developers.openai.com/pt-BR/docs/agent-configuration/agents-md)，访问于 2026-09-19。
