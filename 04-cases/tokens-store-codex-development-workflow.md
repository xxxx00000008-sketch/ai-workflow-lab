# FDE 终态案例：Token 聚合平台的 AI 原生交付系统

> 状态：目标架构，未在 `tokens-store` 真实仓库验证
>
> 编写日期：2026-09-19
>
> 适用环境：Token 聚合平台、Git 仓库、ChatGPT Project、Codex Project、CI/CD、生产监控系统

## 一、最终目标

`tokens-store` 不应只是“一个由 Codex 帮忙写代码的仓库”，而应成为一套由产品事实驱动、由自动化质量门禁约束、由生产指标反馈的交付系统。

FDE 负责的不是单次编码，而是完整结果：

```text
业务问题
  ↓
需求发现与量化目标
  ↓
可执行产品契约
  ↓
系统设计与风险分级
  ↓
Codex 实现、测试和审查
  ↓
CI/CD 门禁与渐进发布
  ↓
生产采用、SLO 和业务指标
  ↓
反馈进入产品、架构和自动化规则
```

终态成功标准不是“功能已经合并”，而是：

- 用户能够在生产环境稳定使用。
- 业务指标或工作流效率发生可测量改善。
- 安全、成本、可靠性和回滚处于可控范围。
- 需求、代码、测试、发布和生产结果可以互相追溯。
- 成功模式被沉淀为规则、Skill、自动化或平台能力。

这与 OpenAI 对 FDE 的定义一致：FDE 负责 discovery、technical scoping、system design、build 和 production rollout，并以生产采用、可测量工作流影响和 eval 驱动反馈衡量结果。

### 1.1 方案依据的优先级

本方案不把 OpenAI 产品文档当作软件工程规范。依据分为三层：

| 层级 | 决定什么 | 采用的依据 |
| --- | --- | --- |
| 业界工程规范 | 安全研发、交付性能、可靠性、供应链和可观测性应该达到什么标准 | DORA、NIST SSDF、OWASP ASVS、SLSA、Google SRE、OpenTelemetry、OpenSSF |
| 产品能力 | ChatGPT、Work、Codex、MCP、Skills、worktree 和计划任务当前可以怎样组合 | 当前 OpenAI 官方产品文档和实际账号验证 |
| 项目约定 | `tokens-store` 如何命名、分级、审批、组织文档和执行任务 | 本项目根据业务风险制定并版本化 |

发生冲突时：

1. 法律、合规和组织安全政策优先。
2. 业界标准决定最低工程要求。
3. OpenAI Docs 只用于确认产品能力、限制和操作入口。
4. 项目约定可以比行业基线更严格，不能用产品便利性降低安全和质量门槛。

本文中的“交付契约”、`DELIVERY-<编号>`、R0–R3 风险等级和 Skill 名称是本方案的项目约定，不是 OpenAI 或通用行业标准。

## 二、当前模式的问题

当前链路大致是：

```text
零散产品文档 → 人临时解释 → Codex 修改 tokens-store → 人工查看结果
```

主要缺口不是代码生成能力，而是工程控制面缺失：

| 缺口 | 直接后果 |
| --- | --- |
| 产品资料没有权威入口 | Codex 可能依据过期或冲突要求实现 |
| 没有可执行需求契约 | “功能完成”无法客观判断 |
| 产品规则与代码之间不可追溯 | 出问题时难以判断是需求、设计还是实现错误 |
| 每个任务临时提示 | 规则重复、遗漏且无法持续改进 |
| 缺少风险分级 | 文档修改与计费、密钥、路由修改采用同样流程 |
| 验证集中在代码测试 | 无法证明真实业务效果和生产可靠性 |
| 没有运行反馈闭环 | 上线后的错误、成本和用户反馈不能改变下一轮开发 |

## 三、FDE 终态架构

```text
┌──────────────────── 产品与客户上下文 ────────────────────┐
│ 产品文档 / 客户反馈 / 工单 / 数据 / 事故 / 商业目标       │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── 产品控制面 ──────────────────────────┐
│ ChatGPT Project + 连接器/MCP                              │
│ 需求发现、冲突整理、术语统一、价值指标、产品决策           │
│                         ↓ 人工批准                         │
│ 仓库内已批准交付契约（Delivery Contract）                  │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── 工程控制面 ──────────────────────────┐
│ AGENTS.md / ADR / API Contract / Risk Policy / Eval Plan  │
│ Issue 状态、依赖、范围、验收标准、发布与回滚计划           │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── Codex 交付面 ────────────────────────┐
│ 一个结果一个任务 / worktree 隔离 / Skills / 有界多代理     │
│ 调查 → 计划 → 实现 → 测试 → 自审 → PR                    │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── 自动质量门禁 ────────────────────────┐
│ Lint / 类型 / 单元 / 契约 / 集成 / E2E / 安全 / 性能      │
│ 需求追溯 / 变更范围 / Secret Scan / Migration Check      │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── 生产交付面 ──────────────────────────┐
│ Feature Flag → Preview → Canary → Production → Rollback   │
└──────────────────────────┬───────────────────────────────┘
                           ↓
┌──────────────────── 运行反馈面 ──────────────────────────┐
│ SLO / 成本 / Token 用量 / 路由质量 / 告警 / 用户采用      │
│ 事故与反馈 → Eval、产品契约、AGENTS、Skill、Roadmap       │
└──────────────────────────────────────────────────────────┘
```

### 系统职责

| 系统 | 唯一职责 | 不承担什么 |
| --- | --- | --- |
| 原始产品系统 | 保存原始 PRD、客户反馈和商业背景 | 不直接作为 Codex 的无筛选上下文 |
| ChatGPT Project | 发现问题、整理多来源资料、澄清产品判断 | 不保存唯一正式需求版本 |
| Git 仓库 | 保存批准后的交付契约、架构决定、代码、测试和运行手册 | 不保存生产密钥和客户敏感数据 |
| Issue/任务系统 | 保存工作状态、责任人、依赖和交付链接 | 不重复保存完整产品事实 |
| Codex Project | 读取权威上下文并完成可审查工程交付 | 不自行决定冲突产品规则或生产放量 |
| CI/CD | 执行确定性验证和发布策略 | 不替代人工业务验收 |
| 可观测系统 | 保存生产行为、SLO、成本和告警事实 | 不用未经脱敏的数据训练提示或模型 |

### 行业规范落地矩阵

| 领域 | 采用基线 | 在 `tokens-store` 中的落点 |
| --- | --- | --- |
| 交付性能 | DORA 当前五指标 | Change lead time、Deployment frequency、Failed deployment recovery time、Change fail rate、Deployment rework rate |
| 可靠性 | Google SRE | 面向用户的 SLI/SLO、Error Budget、Canary、停止条件和回滚策略 |
| 安全研发 | NIST SSDF 1.1 | 保护代码和环境、生成安全软件、响应漏洞、保存可审计证据 |
| 应用安全 | OWASP ASVS 5.0.0 | 按风险选择并引用带版本的安全验收要求，不使用模糊的“已做安全检查” |
| 软件供应链 | SLSA 1.2 | 受保护源码、可追溯构建、Provenance、制品验证和逐级提升保证 |
| 可观测性 | OpenTelemetry | traces、metrics、logs、profiles 和 resources 使用统一语义及关联标识 |
| 仓库安全卫生 | OpenSSF Scorecard | 分支保护、依赖更新、固定依赖、危险工作流和安全政策检查 |

终态必须为每个领域明确采用版本、目标等级、适用控制和验证证据。与实际威胁模型无关的控制可以不采用，但必须记录理由；不能用“项目规模小”作为跳过安全、供应链、可靠性或可观测性基线的理由。

### ChatGPT 与 Codex 产品能力映射

| 工作环节 | 建议产品能力 | 作用 | 权威产物在哪里 |
| --- | --- | --- | --- |
| 长期产品上下文 | ChatGPT Project | 保存产品指令、相关对话、上传资料和连接来源 | 原始资料仍归原系统；批准前分析留在 Project |
| 当前公开信息 | Search / Deep Research（账号支持时） | 查询供应商、标准、市场和外部证据 | 交付契约记录实际采用的来源与日期 |
| 结构化产品产物 | ChatGPT Work（账号和 Project 设置支持时） | 生成或编辑需求简报、报告、表格和评审材料 | 批准后的工程输入必须写入 Git |
| 外部业务上下文 | Apps / Connectors / MCP | 受控读取产品文档、Issue、客户反馈和监控 | 外部系统保持原始事实；Git 保存批准结论 |
| 本地工程交付 | Codex Project | 读取仓库、修改代码、运行命令、测试、Diff 和 Git | `tokens-store` Git 仓库 |
| 并行隔离 | Codex worktree | 每个独立结果使用隔离 checkout | 对应分支、Commit 和 PR |
| 持久工程规则 | `AGENTS.md` / `.codex/config.toml` | 自动加载仓库规则和受控配置 | Git 仓库 |
| 标准作业程序 | Skills | 封装单一职责、可复用的工程方法 | 仓库 `.agents/skills/` |
| 周期工作 | Scheduled Tasks / Automation | 执行稳定的检查、汇总和监控 | 任务结果回到 Issue、Git 或监控系统 |
| 人工验收 | Diff / Review / Browser / Preview | 检查真实变更和实际运行结果 | Review 记录、测试证据和生产指标 |

产品组合原则：ChatGPT负责发现和决策上下文，Codex负责本地工程交付；Git保存批准后的工程事实，生产可观测系统保存运行事实。任何产品中的对话历史都不充当最终系统记录。

## 四、ChatGPT Project 与 Codex Project 如何衔接

### 4.1 先区分两个 Project

建议建立两个逻辑上相关、物理上独立的 Project：

| 项目 | 建议名称 | 实际载体 | 负责什么 |
| --- | --- | --- | --- |
| ChatGPT Project | `tokens-store-product` | ChatGPT 中的云端项目空间 | 产品研究、原始资料、客户反馈、讨论、指标定义和交付契约草稿 |
| Codex Project | `tokens-store` | ChatGPT 桌面端连接的本地 Git 仓库 | 正式交付契约、代码、测试、Git、CI/CD 配置、运行文档和可审查变更 |

两者不是同一个 Project 的两个视图，也不会因为名称相同自动关联。

根据当前官方说明：

- ChatGPT Project 组织 chats、files、instructions 和连接的 sources，本身不直接获得电脑本地目录访问。
- Codex 是独立视图，历史与 ChatGPT 历史分开；本地项目的主文件夹用于 Git 操作以及自动发现 `AGENTS.md`、Skills 和 `.codex` 配置。

因此，本案例后文会分别列出两类结构：

- **ChatGPT Project 逻辑结构**：Project instructions、Chats、Sources、Saved responses 和 Connected apps；它不是电脑文件目录。
- **Codex Project 文件结构**：以本地 Git 仓库 `tokens-store/` 为根的真实目录树。

Issue、CI/CD、可观测性、密钥与权限、外部产品资料等系统也会列出各自的逻辑结构。它们不需要在 ChatGPT Project 或 Git 仓库中复制一套同名目录，而是通过统一标识符和链接衔接。

### 4.2 两个 Project 各自保存什么

```text
ChatGPT Project: tokens-store-product
├── Project instructions              # 产品研究、表达和决策规则
├── Chats                             # 需求发现、访谈分析、方案讨论
├── Sources                           # 上传资料、原始文档、已保存回答
└── Connected sources                 # 获准访问的 Drive、Slack 等来源

                 人工批准 + 受控交接
                           ↓

Codex Project: tokens-store
└── tokens-store/                      # 本地 Git 仓库，也是建议目录树的根
    ├── AGENTS.md
    ├── docs/product/delivery-contracts/
    ├── contracts/
    ├── tests/
    ├── src/
    └── ...
```

ChatGPT Project 中的 Sources 是项目上下文，不是 Git 文件目录。即使把同一份 Markdown 上传到 ChatGPT Project，也只是一个独立副本，不应假定它会随仓库修改自动更新。

### 4.3 唯一权威版本放在哪里

不同信息有不同权威位置：

| 信息 | 权威位置 |
| --- | --- |
| 原始 PRD、客户访谈、聊天和外部资料 | 原始产品系统；ChatGPT Project 保存链接、上传副本或分析上下文 |
| 尚未批准的需求分析和方案 | ChatGPT Project 对话 |
| 已批准的交付契约 | `tokens-store/docs/product/delivery-contracts/DELIVERY-<编号>.md` |
| 架构决定 | `tokens-store/docs/architecture/adr/` |
| API、事件和 Provider 契约 | `tokens-store/contracts/` |
| 代码、测试和部署配置 | `tokens-store` Git 仓库 |
| 任务状态 | Issue/项目管理系统 |
| 生产行为 | 日志、指标、Trace、告警和审计系统 |

交付契约一旦批准并进入 Git，Git 中的版本就是本次工程交付的唯一权威版本。ChatGPT Project 中的草稿或上传副本不得覆盖它。

### 4.4 从 ChatGPT Project 交接给 Codex Project

每次功能交接执行以下步骤：

1. 在 `tokens-store-product` 中读取原始资料，完成问题、范围、产品规则、验收示例、指标、风险、发布和回滚分析。
2. ChatGPT 输出一份带唯一编号的交付契约草稿，例如 `DELIVERY-023`。
3. 产品负责人/FDE 人工确认冲突、范围、指标和风险等级，并将状态改为“已批准”。
4. 打开 Codex Project `tokens-store`，新建一个只负责接收契约的任务。
5. 把已批准契约完整交给 Codex，要求写入 `docs/product/delivery-contracts/DELIVERY-023.md`，此时不修改业务代码。
6. 人工审查 Diff，确认 Git 文件与批准内容一致后提交。
7. 后续功能开发任务只引用仓库中的 `DELIVERY-023.md`，不再引用一整段 ChatGPT 对话。

交接提示示例：

```text
任务：登记已批准交付契约，不实现功能。

契约编号：DELIVERY-023
目标路径：docs/product/delivery-contracts/DELIVERY-023.md
批准状态：已批准
批准人：<负责人>
批准日期：<日期>

要求：
1. 读取 AGENTS.md；
2. 将下方契约原样整理为仓库 Markdown；
3. 检查编号、来源链接、风险等级和验收示例是否完整；
4. 不修改 src、tests、配置或依赖；
5. 完成后只返回 Diff 和缺失字段。

<已批准契约正文>
```

这一步把“对话里的决定”转换为“Git 中可追踪的工程输入”。

### 4.5 从 Codex Project 回流到 ChatGPT Project

代码交付后不要复制整个 Codex 任务历史。Codex 应先在仓库中更新交付契约的结果部分：

- PR、Commit 和发布版本
- 实际实现范围及偏差
- 测试和 Eval 结果
- 灰度与生产指标
- 已知限制和后续决策

然后生成一份简短回流摘要：

```text
交付编号：DELIVERY-023
状态：已发布 / 已回滚 / 未达到目标
实现链接：<PR 或 Commit>
生产结果：<指标与观察周期>
与原计划偏差：<内容>
需要产品决定：<问题>
仓库权威文件：docs/product/delivery-contracts/DELIVERY-023.md
```

把这份摘要保存到 `tokens-store-product` 的对应对话或 Project Source，供下一轮产品分析使用。如果账号和权限允许通过连接器/MCP读取 Git、Issue 或监控数据，可以自动获取最新状态；否则由人复制摘要。无论采用哪种方式，都不改变 Git 和生产监控分别作为工程事实、运行事实权威来源的原则。

### 4.6 完整衔接生命周期

| 状态 | 工作位置 | 输出 | 下一步入口 |
| --- | --- | --- | --- |
| 原始资料 | 产品系统、ChatGPT Project | 来源清单和问题 | ChatGPT 产品分析对话 |
| Draft | ChatGPT Project | 交付契约草稿 | 人工产品审批 |
| Approved | ChatGPT Project → Codex Project | 已批准契约内容 | Codex 写入 Git |
| Recorded | Git 仓库 | `DELIVERY-<编号>.md` | Codex 工程计划 |
| In development | Codex worktree | 代码、测试和 PR | CI 与人工 Review |
| Released | Git、CI/CD | 版本、发布与回滚记录 | 生产观察 |
| Observed | 监控系统、Git | SLO、成本、采用和偏差 | 回流 ChatGPT Project |
| Superseded | Git、产品系统 | 新契约编号和替代关系 | 下一轮交付 |

### 4.7 明确不会自动发生的事情

- ChatGPT Project 不会自动读取本地 `tokens-store` 的最新文件。
- Codex Project 不会自动继承 ChatGPT Project 的对话、Sources 或 Project instructions。
- 上传到 ChatGPT Project 的仓库文件不会与 Git 双向同步。
- Codex 完成代码任务后，不会自动把产品结论写回 ChatGPT Project。
- 两个 Project 使用相同名称，不会建立关联。

两者的稳定关联键不是名称，而是 `DELIVERY-<编号>`、Git 路径、Issue、PR 和发布版本。

## 五、产品事实系统

### 5.1 原始资料与执行契约分离

产品文档可以继续存在于多个系统，但代码实现只能依据一份经过批准并写入 Git 的 **交付契约（Delivery Contract）**。

```text
原始 PRD、聊天、截图、工单、客户反馈
               ↓ 汇总、去重、发现冲突
          已批准交付契约
               ↓ 人工批准
       Codex 可以开始工程计划
```

原始资料是证据，交付契约是本次实现的权威输入。冲突内容不能由 Codex 自行选择。

### 5.2 每份交付契约必须包含

```markdown
# DELIVERY-<编号>：标题

## Problem
谁在什么场景遇到什么问题？当前损失是什么？

## Outcome
上线后哪一个用户行为或业务指标应发生变化？

## Scope / Non-scope
本次做什么，明确不做什么？

## Product rules
路由、权限、配额、计费、重试、失败和兼容规则。

## Interfaces
API、事件、数据结构、状态转换和外部依赖。

## Acceptance examples
使用 Given / When / Then 描述正常、边界和失败场景。

## Risk class
R0 / R1 / R2 / R3，并说明理由。

## Eval and metrics
离线测试、线上指标、基线、目标值和观察周期。

## Rollout and rollback
开关、灰度范围、放量条件、停止条件和回滚方法。

## Sources and decisions
原始资料链接、负责人、批准时间和关键取舍。
```

### 5.3 文档冲突治理

每个产品资料条目至少记录：编号、来源、所有者、日期、状态、影响模块和权威级别。

冲突处理规则：

1. Codex 发现冲突后停止写代码。
2. 输出冲突内容、受影响行为和可选方案。
3. 产品负责人或 FDE 做决定。
4. 决定写入交付契约；架构性取舍同时写入 ADR。
5. 被替代资料标注状态，不删除历史证据。

## 六、Token 平台必须显式建模的领域

以下是终态能力地图，不代表 `tokens-store` 当前一定采用这些模块。第一次仓库调查要把真实代码映射到这些领域：

| 领域 | 必须明确的契约 |
| --- | --- |
| Provider Adapter | 上游认证、模型映射、请求/响应转换、错误标准化、版本兼容 |
| Credential Vault | 加密、访问范围、轮换、吊销、审计、禁止日志字段 |
| Routing Policy | 优先级、权重、健康度、成本、延迟、容量、租户策略 |
| Resilience | 超时、重试、退避、熔断、降级、幂等和重复计费防护 |
| Quota & Rate Limit | 用户、租户、模型、供应商维度的限额与并发规则 |
| Usage Ledger | 请求、Token 用量、价格版本、成本、账单核对和补偿 |
| Access Control | 租户隔离、角色、服务账户、最小权限和高风险审批 |
| Audit | 谁在何时修改了凭据、路由、价格、配额和权限 |
| Observability | 成功率、延迟、首 Token 时间、错误、重试、成本和供应商健康 |
| Control Plane | 配置发布、版本、校验、灰度、回滚和环境隔离 |

任何功能都必须说明影响了哪些领域。跨越 Credential、Usage Ledger、Access Control 或生产路由的改动自动进入最高风险流程。

## 七、风险分级与自动化权限

| 级别 | 典型变更 | Codex 可以做 | 必须人工批准 |
| --- | --- | --- | --- |
| R0 | 文档、测试说明、无行为重构 | 创建 PR、运行全部检查 | 合并 |
| R1 | 管理界面、小型非关键逻辑 | 实现、测试、预览部署 | PR 合并和生产发布 |
| R2 | 路由、限流、重试、Provider Adapter | 计划、实现、沙箱验证、生成发布方案 | 设计、灰度、扩量和回滚决定 |
| R3 | 密钥、鉴权、租户隔离、计费、数据删除、生产配置 | 调查、提出补丁、生成验证证据 | 设计、安全审查、合并、部署和生产操作 |

固定边界：

- Codex 永远不读取、显示或提交生产 Token、密钥和客户数据。
- R2/R3 不允许自动合并或无人值守生产发布。
- 自动化凭据按任务最小化，读写权限分离，并具有明确作用域和过期时间。
- 日志、测试夹具、截图和错误报告必须脱敏。
- 不允许仅凭模型总结跳过 Diff、测试结果和生产指标。

## 八、Codex 工程控制面

### 8.1 `AGENTS.md` 分层

根目录保存全仓库规则：

- 项目目标和领域边界
- 仓库地图
- 安装、启动、测试、lint、构建和迁移命令
- 分支、Commit 和 PR 约定
- 风险分级和审批矩阵
- Token、密钥、日志和客户数据安全规则
- Definition of Done

领域目录仅在规则确实不同时增加更具体的 `AGENTS.md` 或覆盖文件，例如 Provider Adapter、计费或权限模块。根文件保持简短，将详细标准链接到专用文档。

### 8.2 项目配置

仓库级 `.codex/config.toml` 保存团队一致的 Codex 行为，例如批准策略、沙箱、MCP 和多代理配置；个人偏好保存在个人配置中，不写入仓库。

Codex Project 的主文件夹必须是 `tokens-store` Git 根目录，以保证 Git 操作以及 `AGENTS.md`、Skills 和配置的自动发现以正确目录为基准。

### 8.3 Skills 是标准作业程序

终态是一组单一职责、输入输出明确的工程作业程序：

| Skill | 输入 | 输出 |
| --- | --- | --- |
| `discovery-to-contract` | 原始资料和业务问题 | 交付契约草稿、冲突与待批准问题 |
| `feature-delivery` | 已批准交付契约 | 计划、实现、测试和 PR 证据 |
| `bug-investigation` | 现象、环境、日志和期望行为 | 稳定复现、根因和修复选项 |
| `provider-onboarding` | Provider API 与商业规则 | Adapter、契约测试、沙箱结果和上线清单 |
| `routing-change` | 路由策略契约 | 模拟结果、实现、灰度和回滚计划 |
| `security-review` | PR Diff 和安全政策 | 分级发现、证据和最小修复建议 |
| `release-readiness` | 候选版本 | 门禁结果、风险、发布与回滚清单 |
| `incident-response` | 告警和脱敏证据 | 时间线、影响、止损方案和复盘草稿 |
| `docs-drift` | 代码、契约和运行文档 | 不一致列表和建议修改 |

每个 Skill 只负责一个稳定任务；方法放在 Skill，长期约束放在 `AGENTS.md`，本次目标放在交付契约。

### 8.4 MCP 是受控上下文通道

当产品资料、Issue、GitHub、监控或日志在仓库外时，通过受控连接器或 MCP 获取当前数据，避免人工复制过期内容。

每个集成必须定义：

- 允许读取和写入的系统、项目和字段
- 凭据所有者、作用域、轮换和吊销方式
- 哪些操作只读，哪些需要逐次批准
- 敏感字段过滤和审计日志
- 连接失败时的降级方式

不能因为“终态自动化”就给 Codex 全组织、全生产或永久写权限。

## 九、一个需求的终态交付流程

### 9.1 Discovery

FDE 使用 ChatGPT Project 聚合产品资料、客户反馈、事故、指标和工程限制，回答：

1. 谁的问题最值得解决？
2. 当前基线是什么？
3. 成功指标、护栏指标和停止条件是什么？
4. 现有流程中哪一步真正造成损失？
5. 这是产品问题、流程问题还是工程问题？

输出不是代码任务，而是待批准的交付契约。

### 9.2 Readiness Gate

只有满足以下条件才能进入 Codex 实现：

- Problem、Outcome、Scope 和 Non-scope 明确。
- 产品冲突已解决。
- 验收示例覆盖成功、边界和失败路径。
- 风险等级已确认。
- 指标、发布和回滚方案可执行。
- 依赖系统和负责人明确。

不满足时，任务状态是 `blocked-by-contract`，而不是让 Codex 边猜边写。

### 9.3 Engineering Plan

在 Codex 中为该交付契约创建一个独立任务和 worktree，先进入 Plan：

```text
读取 AGENTS.md、DELIVERY-<编号>、相关 ADR、接口契约和代码。

先不要修改文件。输出：
1. 当前行为和证据；
2. 受影响领域、文件、接口、数据和外部依赖；
3. 与交付契约的歧义或冲突；
4. 方案及被放弃的替代方案；
5. 测试、Eval、安全、迁移、发布和回滚计划；
6. 风险等级是否需要调整；
7. 可并行和必须串行的工作。

等待人工批准后再实现。
```

R2/R3 计划需要产品和安全/架构责任人共同批准。

### 9.4 有界并行实现

主任务负责契约、架构一致性和最终整合。仅把相互独立的工作交给子代理，例如：

- 读取现有模块并绘制调用链
- 编写契约测试
- 检查安全和隐私风险
- 检查迁移、兼容和回滚
- 更新运行手册

禁止多个代理同时修改同一核心文件；禁止把产品决策分散给子代理。每个子任务必须返回证据，主任务统一审查。

### 9.5 验证矩阵

| 变更类型 | 必须通过的验证 |
| --- | --- |
| 所有代码 | 格式、lint、类型、单元测试、Secret Scan、Diff 范围检查 |
| Provider Adapter | API 契约测试、错误映射、超时重试、沙箱集成、兼容性 |
| Routing Policy | 固定数据集模拟、确定性规则测试、故障注入、成本和延迟对比 |
| Credential | 加密、权限、轮换、吊销、审计、日志脱敏、安全审查 |
| Usage/计费 | 精度、价格版本、幂等、重复请求、对账和补偿测试 |
| 数据结构 | 前向/后向迁移、回滚、历史数据和并发验证 |
| 性能路径 | 负载、容量、P95/P99、资源和成本预算 |
| 用户流程 | 验收示例、E2E、预览环境和人工业务验收 |

确定性检查由 CI 执行。Codex 可生成和运行测试、做自审与安全审查，但不能用自然语言声称代替测试产物。

### 9.6 PR Gate

PR 必须自动关联：

- 交付契约编号
- 风险等级
- 影响领域
- 验收标准与测试对应关系
- 迁移、发布和回滚方案
- 监控面板与告警变更
- 人工批准记录

CI 失败、契约缺失、R2/R3 审批缺失、Secret Scan 命中或回滚不可行时禁止合并。

### 9.7 渐进发布

```text
Preview / Sandbox
  ↓ 验收和集成测试
内部或指定租户
  ↓ 护栏指标稳定
小比例 Canary
  ↓ SLO、成本、错误和业务指标稳定
逐级扩大
  ↓
全量发布
```

每一级都要定义观察时间、通过阈值和自动/人工停止条件。R2/R3 必须能够通过 Feature Flag、配置版本或发布版本快速回滚。

### 9.8 Production Acceptance

功能只有同时满足以下条件才算完成：

- 生产部署成功且没有触发停止条件。
- SLO 和安全护栏稳定。
- 目标用户实际采用。
- Outcome 指标达到目标或得到可解释结论。
- 运行手册、告警和所有者可用。
- 交付契约记录真实结果与偏差。

如果代码正确但无人使用，或业务指标没有改善，FDE 任务仍未完成。

## 十、Bug 与事故的统一闭环

```text
告警 / 用户反馈
  ↓
影响分级与止损
  ↓
保存脱敏证据和时间线
  ↓
稳定复现或建立观测假设
  ↓
根因，而不是错误表象
  ↓
回归测试 → 最小修复 → 安全与相邻路径检查
  ↓
灰度发布与生产验证
  ↓
复盘：契约、测试、监控、AGENTS 或 Skill 哪一层失效
```

Bug 修复任务必须包含：期望与实际行为、环境、首次发生时间、影响范围、复现步骤、脱敏证据、根因、回归测试、发布和回滚。

严重事故中，优先恢复服务和保护数据；Codex 可以调查、整理时间线和提出操作，但生产切流、凭据轮换、数据修改和删除必须由授权人员批准。

## 十一、运行时可观测与 Eval 系统

### 技术 SLO

- 请求成功率和可用性
- P50/P95/P99 延迟与首 Token 时间
- 超时、重试、熔断和降级比例
- Provider/模型错误分布
- 队列深度、并发和容量余量

### 业务与成本指标

- 活跃租户、功能采用和任务成功率
- 输入/输出 Token 用量
- 每个租户、Provider、模型和请求类型的成本
- 路由节省金额与质量变化
- 配额拒绝、账单偏差和人工补偿量

### 安全指标

- 凭据访问、失败认证和越权尝试
- 敏感字段进入日志或错误消息的检测
- 高风险配置修改和审批情况
- 安全扫描发现、修复时长和重复出现率

### FDE 交付指标

- Time to First Value：从批准契约到首批用户获得价值
- Change lead time：从代码提交到成功运行于生产环境
- Deployment frequency：给定周期内的生产部署次数或部署间隔
- Failed deployment recovery time：失败部署发生后恢复服务所需时间
- Change fail rate：需要立即修复、回滚或干预的部署比例
- Deployment rework rate：因生产问题产生的非计划修复部署比例
- Eval 通过率和生产逃逸缺陷
- 需求到代码、测试、部署和指标的追溯完整率
- 人工介入次数及其真正避免的风险

一般事故的 MTTR 可以继续作为运维指标，但不能替代 DORA 当前定义的 Failed deployment recovery time。

Dashboard 不是展示品。每个告警必须关联负责人和 Runbook，每个交付契约必须声明上线后观察哪些指标。

## 十二、持续自动化

### 事件驱动自动化

| 事件 | 自动动作 | 人工门禁 |
| --- | --- | --- |
| 交付契约进入 Ready | 创建实现任务、worktree 和检查清单 | 批准工程计划 |
| PR 创建/更新 | 测试、契约检查、安全扫描、Codex Review | 合并决定 |
| CI 失败 | 分类失败、关联最近改动、形成修复建议 | 高风险修复批准 |
| 发布候选生成 | 汇总变更、风险、迁移、监控和回滚 | 发布批准 |
| Canary 指标越界 | 自动停止扩量或回滚到安全版本 | 恢复扩量 |
| 生产告警 | 收集脱敏证据、建立事故任务和时间线 | 生产操作 |
| 文档与代码漂移 | 创建报告或草稿 Issue | 修改权威契约 |

### 计划自动化

- 每日：失败 CI、异常成本、Provider 健康、未处理安全告警。
- 每周：过期产品契约、未关闭高风险变更、SLO 和成本趋势、重复 Bug。
- 每次发布：Release Notes、迁移清单、Runbook 和 Dashboard 完整性。
- 每月：权限、凭据、依赖、数据保留和自动化效果审查。

自动化默认生成结构化报告或草稿任务。只有可逆、低风险并有确定性门禁的动作才允许自动执行。

## 十三、建议系统结构（终态示意，无需创建）

这里描述的是整个交付系统，不只是一棵本地目录树：

```text
tokens-store FDE 交付系统
├── ChatGPT Project：tokens-store-product     # 产品发现、研究与决策上下文
├── Codex Project：tokens-store               # 本地 Git 仓库与工程交付
├── Issue / 项目管理系统                      # 工作状态、负责人和审批记录
├── CI/CD 系统                                # 自动验证、构建、发布和回滚
├── 生产可观测系统                            # 指标、日志、Trace、告警和审计
├── 密钥、IAM 与策略系统                      # 凭据、身份、权限和轮换
└── 外部产品事实系统                          # PRD、客户反馈、供应商资料和分析数据
```

只有 **Codex Project** 小节是真实文件目录。其他小节是各产品或系统中的逻辑内容结构，不要求在本地创建同名目录。

### 13.1 ChatGPT Project：产品上下文的逻辑结构

建议项目名：`tokens-store-product`。

```text
ChatGPT Project：tokens-store-product
├── Project instructions
│   ├── 项目目标、产品边界和角色定义
│   ├── 事实、推断、建议的标注规则
│   ├── 输出必须携带 DELIVERY 编号和来源
│   └── 禁止写入密钥、个人数据和未脱敏生产数据
├── Chats
│   ├── [DISCOVERY][DELIVERY-023] 问题与用户证据
│   ├── [DECISION][DELIVERY-023] 范围、规则与指标
│   ├── [CONTRACT][DELIVERY-023] 交付契约草稿
│   ├── [REVIEW][DELIVERY-023] 发布前产品复核
│   ├── [OUTCOME][DELIVERY-023] 生产结果与偏差
│   └── [INCIDENT][INC-xxx] 事故分析与后续决定
├── Project sources
│   ├── [SOURCE] PRD、研究报告和脱敏访谈材料
│   ├── [REFERENCE] 术语表、产品规则和标准
│   ├── [SNAPSHOT][DELIVERY-023] 已批准契约只读副本
│   ├── [OUTCOME][DELIVERY-023] 交付结果摘要
│   └── Saved responses：可复用摘要、决策记录和分析结果
├── Connected apps / links
│   ├── 产品文档系统
│   ├── 客户反馈或沟通系统
│   ├── Issue / 项目管理系统
│   └── 指标与 Dashboard（优先只读）
└── Project memory and sharing
    ├── 按数据边界选择 project-only memory
    ├── 按最小权限授予 chat 或 edit access
    └── 定期复核成员、Sources 和失效材料
```

ChatGPT Project 当前并不提供上述文件夹层级；这里用前缀、编号和“一次结果一条 Chat”的方式形成可检索结构。Project source 是上下文副本或链接，不是权威 Git 文件，也不会自动与本地仓库同步。Google Drive、Slack 等连接能力受账号、工作区设置和应用支持范围约束。

### 13.2 Codex Project：本地 Git 仓库的真实目录

建议项目名：`tokens-store`；主文件夹就是 clone 后的本地 Git 仓库 `tokens-store/`。

```text
tokens-store/
├── AGENTS.md                              # 全仓库 Agent 规则与完成标准
├── README.md                              # 项目入口、运行与关键链接
├── .codex/
│   └── config.toml                        # 团队级 Codex 配置与受控集成
├── .agents/
│   └── skills/
│       ├── discovery-to-contract/
│       ├── feature-delivery/
│       ├── bug-investigation/
│       ├── provider-onboarding/
│       ├── routing-change/
│       ├── security-review/
│       ├── release-readiness/
│       ├── incident-response/
│       └── docs-drift/
├── docs/
│   ├── product/
│   │   ├── index.md                       # 原始资料索引和权威状态
│   │   ├── glossary.md                    # 统一领域语言
│   │   ├── delivery-contracts/            # 已批准的交付契约
│   │   └── decisions/                     # 产品规则决定
│   ├── architecture/
│   │   ├── system-context.md
│   │   ├── domain-map.md
│   │   ├── data-flow.md
│   │   ├── threat-model.md
│   │   └── adr/                           # 架构决策记录
│   ├── operations/
│   │   ├── slo.md
│   │   ├── dashboards.md
│   │   ├── alerts.md
│   │   └── runbooks/
│   └── security/
│       ├── credential-policy.md
│       ├── data-classification.md
│       └── approval-matrix.md
├── contracts/
│   ├── api/                               # API/OpenAPI 等契约
│   ├── events/                            # 事件契约
│   └── providers/                         # 上游 Provider 契约样本
├── tests/
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   ├── e2e/
│   ├── performance/
│   ├── security/
│   └── evals/                             # 路由和产品行为固定评测集
├── observability/
│   ├── dashboards/
│   ├── alerts/
│   └── redaction/                         # 日志脱敏规则
├── deploy/
│   ├── environments/
│   ├── migrations/
│   ├── feature-flags/
│   └── rollback/
├── src/                                   # 真实名称按现有项目调整
└── .github/                               # CI、PR 模板和安全门禁
```

该目录树表达最终责任边界，不要求为了文档创建空目录或占位文件。真实仓库可按现有技术栈合并或改名，但不能丢失对应职责。

### 13.3 Issue / 项目管理系统：交付状态结构

```text
Delivery board
├── Backlog
├── Discovery
├── Contract review
├── Ready
├── In development
├── PR review
├── Canary
├── Observed
├── Done
└── Rejected / Superseded

每个交付项
├── DELIVERY 编号与标题
├── 问题、范围、成功指标和风险等级
├── Owner、审批人和目标时间
├── 权威契约、PR、Release、Dashboard 链接
├── 当前门禁与阻塞项
└── 结果、偏差和后续决定
```

Issue 系统负责状态，不复制完整契约正文。契约正文仍以 Git 中 `docs/product/delivery-contracts/` 的批准版本为准。

### 13.4 CI/CD 系统：流水线结构

```text
Pull request pipeline
├── 校验 DELIVERY 编号、契约链接和变更范围
├── Format / Lint / Type check / Unit test
├── Contract / Integration / E2E / Eval
├── Security scan / Dependency scan / Secret scan
├── SBOM / Provenance / Artifact build and sign
└── Preview environment + 人工 Review

Release pipeline
├── Release candidate
├── 数据迁移和回滚预检
├── Canary 发布
├── SLO、错误率、成本和业务指标门禁
├── 分批扩量或自动停止
├── Production 发布
└── 发布验证、回滚记录和 Outcome 回写
```

### 13.5 生产可观测系统：运行事实结构

```text
Observability
├── Dashboards
│   ├── SLO / SLA
│   ├── Provider 健康与配额
│   ├── 路由质量、降级和回退
│   ├── Token 用量、成本和毛利
│   └── 安全、租户隔离和审计
├── Alerts
│   ├── Page：立即影响用户或安全
│   ├── Ticket：需要排期修复
│   └── Record：仅记录趋势
├── Telemetry
│   ├── Metrics
│   ├── Logs（脱敏）
│   ├── Traces
│   └── Audit events
└── Runbooks
    ├── Provider 故障
    ├── 路由异常
    ├── 成本异常
    ├── 凭据泄露或失效
    └── 回滚与事故响应
```

### 13.6 密钥、IAM 与策略系统：安全控制结构

```text
Security control plane
├── Secret vault
│   ├── Provider credentials
│   ├── Environment-scoped secrets
│   └── Rotation and revocation
├── Identities
│   ├── Human accounts
│   ├── Service accounts
│   └── CI/CD identities
├── Roles and policies
│   ├── Development
│   ├── Review and approval
│   ├── Release
│   └── Production break-glass
└── Audit
    ├── Access records
    ├── Privilege changes
    ├── Secret usage
    └── Periodic access review
```

任何真实密钥都不得写入 ChatGPT Project、Codex 对话、Git 文档、Issue 或日志。上述位置只保存密钥引用、负责人、用途、轮换策略和审计链接。

### 13.7 外部产品事实系统：原始资料结构

```text
External systems of record
├── 产品文档：PRD、路线图、规则和定价
├── 客户事实：访谈、工单、反馈和销售记录
├── Provider 事实：官方文档、状态、配额和变更公告
├── 经营分析：采用率、收入、成本和留存
├── 事故与支持：Incident、Postmortem 和已知问题
└── 合规资料：数据分类、合同、保留和审计要求
```

原始资料留在各自权威系统。ChatGPT Project 通过上传的脱敏副本、支持的应用链接或受控连接读取；批准后的工程结论进入 Git，不把全部原始资料搬进仓库。

### 13.8 跨系统关联键

| 标识符 | 作用 | 必须出现的位置 |
| --- | --- | --- |
| `DELIVERY-023` | 一次产品结果的主关联键 | ChatGPT Chat、Project source、Issue、Git 契约、分支/PR、Release、Dashboard 注释 |
| Issue 编号 | 跟踪状态、Owner 和审批 | Git 契约、PR、Release |
| PR / Commit | 定位实际工程变更 | Issue、交付契约结果、Release |
| Release / Deployment ID | 定位已发布版本 | Git tag、CI/CD、Dashboard、Outcome 摘要 |
| Trace / Correlation ID | 追踪单次运行 | 日志、Trace、事故记录；仅保存脱敏值 |
| Incident ID | 关联生产事故与改进 | 监控、Issue、交付契约后续项、ChatGPT 复盘 Chat |

完整链路应能从任一 `DELIVERY` 编号跳转到产品讨论、批准契约、工程改动、发布记录和生产结果。关联靠编号与链接，不靠 ChatGPT Project 和 Codex Project 自动同步。

## 十四、人工决策点

无论自动化程度多高，以下事项保持人工责任：

- 业务问题、优先级和成功指标
- 冲突产品规则的最终决定
- R2/R3 设计和安全审批
- 新生产依赖和不可逆数据迁移
- 密钥、权限、计费、数据删除和租户隔离变更
- Canary 扩量、生产发布和事故中的高风险操作
- 接受未达到 Outcome 指标的产品结果

Codex 提供证据、选项和执行能力；FDE 对结果、权衡和生产影响负责。

## 十五、终态验收标准

只有同时满足以下条件，`tokens-store` 才达到本方案定义的终态：

1. 每个生产变更都能追溯到已批准的交付契约。
2. 产品资料冲突有明确所有者和决定记录。
3. `AGENTS.md`、配置和 Skills 使不同任务遵循一致工程规则。
4. 所有写入任务隔离运行，并通过与风险匹配的自动门禁。
5. R2/R3 变更具备安全审查、灰度、停止条件和回滚能力。
6. 生产 SLO、成本、业务采用和安全指标可以观测。
7. 事故和用户反馈会更新 Eval、契约、测试或作业程序。
8. 自动化权限最小、可审计、可撤销，高风险操作有人负责。
9. 功能完成以生产采用和可测量价值为准，而不是以代码合并为准。
10. 新 Provider、新路由策略和常见 Bug 能通过标准流程重复交付。

## 十六、适用限制

- 本文是 FDE 目标操作系统，不是对 `tokens-store` 当前架构的事实描述。
- 尚未读取真实代码、CI、部署、监控、产品资料和组织权限，因此状态保持“未验证”。
- Provider、计费和租户模型需根据真实业务删除或调整，不能直接照搬名称。
- Codex Security、云端 Review、MCP 和计划任务的可用性取决于账号、工作区、平台和权限；启用前需按当前官方文档及实际环境核验。

## 依据与来源

### 业界标准与实践

- [DORA software delivery performance metrics](https://dora.dev/guides/dora-metrics/)：当前五指标模型，覆盖交付吞吐和不稳定性。
- [NIST SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)：将安全开发实践集成进 SDLC，减少漏洞并处理根因。
- [OWASP ASVS](https://owasp.org/projects/asvs)：当前稳定版 5.0.0，为应用安全技术控制提供可引用的验证要求。
- [SLSA 1.2](https://slsa.dev/spec/v1.2/)：源码、构建、Provenance 和制品验证的供应链安全规范。
- [Google SRE — Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)：从用户关心的行为定义 SLI、SLO 和 Error Budget。
- [Google SRE — Canarying Releases](https://sre.google/workbook/canarying-releases/)：通过受限流量和观察窗口降低发布风险。
- [OpenTelemetry Semantic Conventions](https://opentelemetry.io/docs/concepts/semantic-conventions/)：统一 traces、metrics、logs、profiles 和 resources 的命名与语义。
- [OpenSSF Scorecard](https://openssf.org/scorecard/)：自动检查仓库和开源供应链的安全实践。

### ChatGPT 与 Codex 产品能力

- [OpenAI Forward Deployed Engineer](https://openai.com/careers/forward-deployed-engineer-seoul-seoul-south-korea/)：FDE 端到端负责发现、范围、系统设计、构建、生产发布、采用和可测量影响。
- [Projects in ChatGPT](https://help.openai.com/en/articles/10169521-using-projects-in-chatgpt)：ChatGPT Project 组织 chats、files、instructions 和连接来源。
- [ChatGPT Work and Codex](https://help.openai.com/en/articles/20001275/)：Work 面向知识工作交付物，Codex 面向本地仓库、代码、测试和开发工具；两者入口与历史边界不同。
- [Codex Projects](https://developers.openai.com/docs/projects)：本地项目主文件夹以及 `AGENTS.md`、Skills、配置和 Git 的发现边界。
- [Codex 最佳实践](https://developers.openai.com/guides/best-practices)：使用明确上下文、`AGENTS.md`、测试与审查、MCP、Skills、计划任务、worktree 和有界多代理建立稳定工程系统。
- [Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform)：Agent 系统需要上下文、工具、失败处理、审批、状态和结果回传，而不只是提示词与模型回答。
- [Codex Worktrees](https://developers.openai.com/zh-Hans/docs/environments/git-worktrees)：同一仓库中的隔离并行任务和 Local/Worktree 移交。
- [Codex Security](https://developers.openai.com/docs/security)：对仓库、Diff 和 PR 进行安全发现、审查和修复；具体能力需具备相应访问权限。
