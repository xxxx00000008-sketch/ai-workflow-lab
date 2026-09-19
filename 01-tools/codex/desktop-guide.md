# Codex 桌面端：从零到一次可审查交付

> 状态：第一轮完成
>
> 最近验证：2026-09-19
>
> 环境：Windows ChatGPT 桌面应用中的 Codex、Git 仓库

这篇教程不以“把所有按钮点一遍”为目标。你会用 Codex 完成一个最小但完整的闭环：打开项目、定义任务、让 Agent 修改文件、检查验证结果、审查 Diff，并把成果保存在 Git 分支中。

## 完成后你会理解什么

- Project、Thread、Local、worktree 和 Git 分支分别解决什么问题。
- 为什么 `AGENTS.md` 比每次重复粘贴规则更可靠。
- Codex 如何通过文件、终端和测试获得反馈。
- 哪些操作可以委托，哪些必须由你审查或批准。
- 怎样把一次成功任务升级成可复用 Skill 或 Automation。

## 一、先判断是否应该使用 Codex

如果你只是想获得解释、建议或短草稿，使用 Chat 即可。如果任务需要多来源研究并产出报告、表格或演示文稿，优先考虑 Work。

当任务满足以下任一条件时，Codex 更合适：

- 需要读取或修改本地项目文件。
- 需要运行测试、构建、格式化或终端命令。
- 需要检查 Git Diff、提交分支或创建 Pull Request。
- 任务需要持续较长时间或与其他任务并行。

官方快速入门将桌面应用定位于项目、本地文件和较长任务，并建议软件开发工作使用 Codex。[OpenAI Docs](https://learn.chatgpt.com/pt-BR/docs/quickstart)

## 二、建立正确的 Project

### 1. 打开项目目录

在桌面应用中选择 Codex，打开包含实际工作文件的文件夹。对于 Git 项目，优先打开仓库根目录，这样 Git 状态、`AGENTS.md` 和项目配置更容易被正确发现。

### 2. 多文件夹项目

如果应用、后端和文档分散在不同目录，可以把多个文件夹附加到同一 Project。应指定一个主文件夹：新 Thread 默认从这里开始，Git 操作以及 `AGENTS.md`、Skills 和 `config.toml` 的自动发现也以它为主。无关工作应拆成不同 Project，避免权限和上下文过宽。[OpenAI Docs](https://developers.openai.com/es-419/docs/projects?surface=web)

### 3. 一个 Thread 对应一个成果

推荐：

- “为登录接口补充过期令牌测试”
- “整理 Codex 桌面端入门教程”
- “审查支付迁移的兼容性风险”

不推荐：

- “继续”
- “修一下项目”
- “所有事情都做完”

清晰的成果边界让 Diff 更容易审查，也便于以后搜索、归档或分叉。

## 三、选择 Local 还是 Worktree

### 使用 Local

适合：

- 只读调查。
- 很小且确定的修改。
- 必须使用当前未提交改动。
- 需要立即在现有开发环境中人工配合。

风险是 Codex 和你会操作同一工作目录。开始前必须检查未提交变更，避免覆盖无关工作。

### 使用 Worktree

适合：

- 与当前工作并行的新功能。
- 不确定是否采用的实验。
- 长时间运行的任务。
- 多个 Agent 同时处理同一仓库中的独立工作。

worktree 为 Thread 提供独立 checkout，但共享仓库的 Git 元数据。Codex 可以把 Thread 在 worktree 和 Local 之间 Handoff；被 `.gitignore` 忽略的文件通常不会随交接移动，因此依赖和环境准备需要单独设计。[OpenAI Docs](https://developers.openai.com/es-419/docs/environments/git-worktrees)

### 简单选择法

> 不确定时，调查用 Local，写入用 worktree；必须包含当前未提交状态时再选 Local。

## 四、准备 AGENTS.md

`AGENTS.md` 应告诉 Codex如何在这个仓库工作，而不是描述本次任务。

一个最小版本可以包含：

```markdown
# AGENTS.md

## Purpose

这是一个中文优先的知识库。

## Working rules

- 修改前阅读 README 和 ROADMAP。
- 不虚构来源或测试结果。
- 使用小写 kebab-case 文件名。

## Validation

- 检查相对链接。
- 检查格式和 Git Diff。
```

稳定规则进入 `AGENTS.md`；一次性目标和限制留在当前提示中。子目录有特殊要求时，在更靠近代码的位置添加更具体的规则。详细说明见 [`AGENTS.md`](agents-md.md)。

## 五、写出可执行的任务

高质量任务至少包含五部分：

```text
目标：补齐 Codex 桌面端入门教程。
范围：只修改 01-tools/codex 和相关索引。
来源：优先使用当前官方 OpenAI Docs。
完成标准：包含 Local/worktree、AGENTS.md、验证和 Git 审查流程。
验证：检查相对链接、Markdown 格式和 Git Diff。
```

对高风险动作增加明确边界：

```text
不要删除现有内容，不要修改 main，不要发布或合并 PR。
遇到需要登录、发送、删除或扩大权限的操作时先询问。
```

## 六、监督 Agent 工作

Codex 的典型循环是：

```text
读取状态 → 制定方法 → 编辑 → 运行工具 → 观察结果 → 修正 → 验证
```

你不需要盯着每一条命令，但需要关注以下信号：

- 是否读取了正确目录和规则。
- 是否把计划误认为已经存在的事实。
- 是否开始修改范围外文件。
- 测试失败后是否真正定位原因。
- 是否请求了超出任务所需的权限。

如果方向错误，直接在当前 Thread 中纠正。新消息应说明新证据或新边界，而不是只说“重来”。

## 七、使用终端和项目 Actions

终端适合一次性诊断和未被界面覆盖的命令。重复使用的启动、测试或构建命令，可以配置为项目 Action。

本地环境配置可以保存在项目根目录的 `.codex` 中，供团队共享；新 worktree 创建时可运行相应 setup script。不要把密钥提交到配置文件中。[OpenAI Docs](https://developers.openai.com/es-419/docs/environments/local-environment)

## 八、审查 Diff，而不是只看总结

任务完成后检查：

1. 改动文件是否都属于本次范围。
2. 是否发生意外删除或大面积机械改写。
3. 新增事实是否有来源或实验依据。
4. 测试和校验是否真的运行并通过。
5. 是否混入密钥、私人信息或临时文件。
6. 提交信息是否说明了实际结果。

桌面端 Diff 面板可查看当前 checkout 的变化、添加行内评论、分块 stage 或 revert、提交、推送并创建 PR。[OpenAI Docs](https://developers.openai.com/es-419/docs/environments/local-environment)

对内容或视觉产物，还要打开实际预览；代码能生成文件不代表排版正确。

## 九、完成 Git 交付

推荐流程：

```text
独立分支或 worktree
        ↓
运行测试与校验
        ↓
审查 Diff
        ↓
Commit
        ↓
Push
        ↓
Pull Request
        ↓
人工 Review 与 Merge
```

不要因为 Codex 能直接提交或创建 PR，就省略人工 Review。Agent Review 也是补充审查者，不替代 CI、分支保护和负责人批准。

## 十、把一次任务升级成系统

完成后复盘：

- 只在这次有效的要求：留在 Thread。
- 每个仓库都应遵守的规则：更新 `AGENTS.md`。
- 经常重复的步骤和模板：创建 Skill。
- 需要连接外部系统并分享给他人：创建 Plugin。
- 固定频率、输入稳定的任务：创建 Automation。

升级的顺序应是：先人工跑通，再标准化，最后自动化。不要自动化一个尚未稳定的过程。

## 十一、第一次练习

在一个可恢复的测试仓库中完成以下任务：

1. 创建 `codex/first-delivery` 分支或 worktree。
2. 让 Codex 阅读 README 和 `AGENTS.md`。
3. 要求它修复一处小型文档问题并补充验证。
4. 查看终端输出和 Diff。
5. 添加一条行内意见并让 Codex修订。
6. 运行最终检查并提交。
7. 不合并，先复盘整个过程。

完成标准不是“Codex 写了文件”，而是你能解释每项变更、验证证据和权限边界。

## 常见错误

| 错误 | 后果 | 改进 |
| --- | --- | --- |
| 一个 Thread 包含多个无关目标 | 上下文混乱，Diff 难审 | 一个成果一个 Thread |
| 在脏工作区直接大改 | 覆盖用户变更 | 先检查状态，必要时使用 worktree |
| 把所有规则塞进提示 | 每次重复且容易漂移 | 稳定规则写入 `AGENTS.md` |
| 只看最终总结 | 漏掉范围外修改 | 审查 Diff 和验证输出 |
| 一开始就做 Automation | 错误被周期性放大 | 先手动跑通，再自动化 |
| 授予过宽权限 | 扩大安全影响 | 最小权限、逐步审批 |

## 下一步

- 理解隔离执行：[`本地、云端与 Worktree`](environments-and-worktrees.md)
- 建立团队规则：[`AGENTS.md`](agents-md.md)
- 复用方法：[`Skills 与 Plugins`](skills-and-plugins.md)
- 自动运行：[`Automations`](automations.md)
- 完整工作流：[`Codex 桌面端交付闭环`](../../03-workflows/codex-desktop-delivery.md)

## 官方来源

1. [Quickstart — ChatGPT Learn](https://learn.chatgpt.com/pt-BR/docs/quickstart)，访问于 2026-09-19。
2. [Projects and chats — OpenAI Docs](https://developers.openai.com/es-419/docs/projects?surface=web)，访问于 2026-09-19。
3. [Worktrees — OpenAI Docs](https://developers.openai.com/es-419/docs/environments/git-worktrees)，访问于 2026-09-19。
4. [Local environments — OpenAI Docs](https://developers.openai.com/es-419/docs/environments/local-environment)，访问于 2026-09-19。
