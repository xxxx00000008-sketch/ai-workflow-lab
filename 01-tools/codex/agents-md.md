# AGENTS.md

`AGENTS.md` 是交给 Agent 的仓库级工作说明，适合保存目录结构、构建测试命令、工程约束、验证标准和禁止事项。它不是面向最终用户的 README，也不应堆积临时任务描述。

## 作用域

Codex 会从全局位置和仓库目录层级读取适用的指导；更靠近当前目录的文件更具体。`AGENTS.override.md` 可在相应层级覆盖常规文件。具体发现规则和大小限制应以当前 OpenAI Docs 为准。

## 推荐内容

- 项目目的与关键目录。
- 构建、测试、lint 和格式化方式。
- 文件命名、架构与提交约定。
- 不得修改或不得执行的事项。
- 完成标准和验证方式。
- Code Review Rules。

规则应短、具体、可执行。只有在 Agent 重复犯错或团队规则稳定时才加入；临时要求留在当前 Thread。

## 来源

- [AGENTS.md instructions — OpenAI Docs](https://developers.openai.com/pt-BR/docs/agent-configuration/agents-md)，访问于 2026-09-19。
