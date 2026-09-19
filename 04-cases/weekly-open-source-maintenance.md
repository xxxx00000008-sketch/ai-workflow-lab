# 真实案例：用 Codex 完成开源项目周维护

> 状态：已验证
>
> 日期：2026-09-19
>
> 项目：`ai-workflow-lab`
>
> 环境：Windows ChatGPT 桌面应用中的 Codex、本地 Git、GitHub 远端

## 真实输入

本案例不是假设。执行时，仓库的 `main` 只有一个初始 README；工作分支经过五次提交，逐步形成 ChatGPT、Codex、工作流、实验和开源协作结构。

执行前可观察状态：

- 远端仓库：`xxxx00000008-sketch/ai-workflow-lab`。
- 基础分支：`main`。
- 工作分支：`codex/v0.1-initialization`。
- 已建立 `AGENTS.md`、Roadmap、Issue/PR 模板和主题文档。

## 传统维护方式

维护者需要手工完成：

1. 浏览所有本周修改。
2. 找出未完成路线图项目。
3. 检查 Markdown 链接。
4. 检查格式、占位符和敏感信息。
5. 整理本周变更。
6. 写提交、推送并准备 PR。
7. 记录下周任务。

这些步骤本身并不难，但容易遗漏，而且每周都会重复。

## 本次 Codex 协作流程

### 用户目标

```text
按开源项目方式维护 ai-workflow-lab，补齐 ChatGPT 和 Codex 的实用内容。
```

### Codex 执行

1. 读取 `README.md`、`ROADMAP.md` 和 `AGENTS.md`。
2. 检查真实工作区和 Git 远端，而不是假设规划已经实现。
3. 实时查阅官方 OpenAI Docs。
4. 在独立分支创建知识卡、工作流、案例和实验。
5. 检查相对链接、Markdown 格式、占位词和敏感信息。
6. 生成清晰提交并推送远端。
7. 保留无法验证的订阅、权限和发布功能为待验证项。

### 实际提交

```text
61337db chore: initialize v0.1 research framework
749f817 docs: complete ChatGPT Projects research loop
5a577c0 docs: complete ChatGPT and Codex foundations
86356ae docs: add Codex desktop delivery guide
```

这说明 Codex 的角色不是“代写一篇文章”，而是把读取状态、研究、编辑、校验和版本控制串成完整交付循环。

## 人工仍然负责什么

- 决定知识库的商业定位和目标读者。
- 判断内容是否真的帮助第一次使用的人。
- 纠正“真实案例”被误解成构建网站的方向偏差。
- 决定是否发布、合并或删除成果。
- 审查官方资料是否与自身账号实际界面一致。

这次真实偏差证明：自动验证可以发现坏链接，却不能判断交付方向是否符合用户真正想要的学习方式。业务验收不能被技术校验替代。

## 适合进一步自动化的步骤

可以建立每周只读 Automation：

```text
每周五检查 ai-workflow-lab：
1. 找出 ROADMAP 中未完成项目；
2. 检查 Markdown 相对链接；
3. 找出 TODO、TBD、FIXME 和超过 90 天未验证的产品说明；
4. 汇总本周提交和变更文件；
5. 输出下周候选任务，按影响和依赖排序。
无异常且无实质变化时保持安静。
不要自动修改、提交、推送或创建 Issue。
```

经过人工确认后，可以逐步增加：自动创建草稿 Issue、生成周报草稿或提醒过期文档。正式内容修改和合并仍保留人工审批。

## 建议目录结构（无需创建）

```text
project/
├── README.md             # 项目入口与当前阶段
├── ROADMAP.md            # 未完成任务与优先级
├── CHANGELOG.md          # 已发布的重要变化
├── AGENTS.md             # Codex 执行规则
├── docs/                 # 正式内容
├── experiments/          # 验证记录
├── templates/            # 周报和检查模板
└── .github/              # Issue 与 PR 模板
```

这是周维护自动化所需信息的逻辑位置示例。案例本身不要求新建这些目录或占位文件，应映射到现有仓库结构。

## 传统、协作、自动化对比

| 环节 | 传统 | Codex 协作 | Automation |
| --- | --- | --- | --- |
| 查当前状态 | 人工逐文件查看 | Codex 搜索仓库并汇总 | 定时扫描并只报告变化 |
| 查官方更新 | 人工逐页搜索 | 实时检索并打开官网 | 定时监测指定官方来源 |
| 内容修改 | 手工编辑 | Codex 按规则批量编辑 | 不建议无审查自动改正式内容 |
| 链接和格式 | 人工抽查 | 命令全量检查 | 每周自动检查 |
| 提交与 PR | 手工执行 | Codex 准备并推送分支 | 只生成候选，不自动合并 |
| 方向判断 | 人负责 | 人纠偏 | 不自动化 |

## 衡量是否提效

以后每周记录四个数字：

- 人工准备时间。
- Codex 执行与等待时间。
- 人工审查时间。
- 因方向或事实错误造成的返工时间。

只有总耗时下降且返工、风险没有增加，才算真正提效。

相关手册：[`ChatGPT + Codex 实际工作提效手册`](../03-workflows/chatgpt-codex-productivity-playbook.md)。
