# OpenSpec 三阶段交付法

> 状态：A2 方法方案，正在“差价 AI”中验证  
> 适用对象：第一次使用 AI Agent 交付软件项目的新成员

## 只记住三个阶段

```text
1. 定义：proposal + specs       → 确认做什么、怎样验收
2. 方案：design + tasks         → 确认怎样做、按什么顺序做
3. 交付：apply + verify/archive → 实现、验收、合入当前事实
```

四类 OpenSpec 文件不是四轮项目审批。`proposal + specs` 一起过需求门禁，`design + tasks` 一起过实施门禁，最终产品一起过验收门禁。

客户交付契约、目标 Demo、调研和现状基线都是第一阶段的输入，不再各自占一个流程步骤。排期写进 tasks 的依赖和里程碑；运维手册是交付物，也不单独增加阶段。

## 最小目录

```text
openspec/
├── specs/                         # 已实现、已验收的当前事实
└── changes/<change-id>/           # 当前要做的一项变化
    ├── .openspec.yaml
    ├── proposal.md                # 为什么做、改什么
    ├── specs/
    │   └── <capability>/spec.md   # 可验证行为
    ├── design.md                  # 技术方案
    └── tasks.md                   # 实施清单
```

一个小型 MVP 默认使用 1–3 个 capability。只有能够独立理解、验证或长期演进时才拆分，不能为了“看起来规范”把每个页面和模块都变成一个 spec。

OpenSpec 官方模型是 `proposal → specs → design → tasks`，并把完成的 delta specs 合入当前 specs；参见 [Overview](https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md)、[Getting Started](https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md) 和 [Writing Specs](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)。极客时间第 [17](https://time.geekbang.org/column/article/935801)、[18](https://time.geekbang.org/column/article/936334) 讲作为需求澄清和任务编译的补充参考；本仓库不复制未公开正文。

## 阶段 1：定义

### 输入

- 客户已确认的目标、范围、非目标和验收方式；
- 当前系统事实、目标 Demo 和必要证据；
- 会改变用户结果的待确认问题。

### 先完成核心链路澄清

AI 不得根据“做一个网站”“做一个后台”直接推断需求。生成 proposal/specs 前，必须向负责人提出并关闭会改变产品形态的问题：

1. 谁在什么触发下完成什么核心任务，最终业务结果是什么？
2. 数据或内容从哪里进入系统，是否持续变化？
3. 每个关键步骤由人完成、由系统自动完成，还是需要人工批准？
4. 更新频率、新鲜度和手动触发要求是什么？
5. 正常链路、失败链路、恢复链路分别是什么？
6. 外部依赖不可用时，用户看到什么，系统保留什么？
7. 交付团队离场后，谁负责正常运行，谁只处理异常？
8. 哪些动作涉及权限、资金、隐私、合规或不可逆风险？

不同行业给出的答案不同；工作流固定的是“必须问清”，不是固定使用爬虫、后台、人工审批或某个技术栈。动态数据项目如果没有明确来源、更新、失败和离场运行方式，需求门禁不得通过。

### AI 执行动作

1. 建立一个动词开头的 change，例如 `deliver-chajia-mvp`。
2. 生成简短 `proposal.md`：`Why`、`What Changes`、`Capabilities`、`Impact`。
3. 默认合并相近行为，只保留 1–3 个 capability。
4. 为每个 capability 写 delta spec：`Purpose`、`ADDED/MODIFIED/REMOVED`、`Requirement`、`Scenario`。
5. 每条 requirement 使用 SHALL/MUST 表达一个可观察行为，并至少有一个 WHEN/THEN 场景。

### 不允许写入 spec

框架、数据库、表名、API 路径、类名、代码目录和实施步骤。更换技术但外部行为不变的内容属于 design。

### 需求门禁 A

- 产品负责人/客户能判断每个场景通过或失败；
- 正常、空、错、过期和无权限状态没有关键缺口；
- 范围、非目标、requirements 和 scenarios 没有冲突；
- 当前事实、推断和待确认项没有混写；
- 核心业务链路、数据生命周期、自动化边界、失败行为和离场运行方式已确认；
- `openspec validate <change-id> --strict` 通过，或明确记录 CLI 尚未验证。

通过后一次性批准 `proposal + specs`，不逐文件审批。

## 阶段 2：方案

### AI 执行动作

1. 读取批准的 proposal/specs 和真实仓库，生成 `design.md`。
2. 目标 Demo 只约束可观察体验和验收行为，除非客户明确要求接管既有生产系统，否则不得把 Demo 的框架、托管方式或页面源码当作技术栈继承依据。
3. design 负责技术架构方案，不重复 spec。至少包含：目标与约束、现状证据、候选方案、最终技术选型、系统/数据/部署架构、关键正常与失败时序、安全、可观察性、性能容量、扩展阈值、测试、迁移和首个垂直切片。
4. 新项目没有既有技术栈时，必须比较真实候选方案，并明确语言、框架、运行时、数据库、异步任务、部署和测试选型及否决原因；不得直接写一个技术名称后开始编码。
5. 既有项目先遵守现有约束，说明本次架构增量；需要更换技术时必须证明收益和迁移路径。
6. 对持续变化的数据回答：来源是什么、怎样产生和更新、自动化与人工边界、失败怎样降级、历史与当前值怎样管理、离场后怎样运行。
7. 为性能和可扩展性定义容量模型、待验证指标、缓存/索引/并发策略、瓶颈、架构上限和触发升级的可观察阈值；没有基线时明确要求先测，不能编造 SLO。
8. 将 design 编译为 `tasks.md`；每项使用 `- [ ] X.Y`，按依赖排序，并在任务描述中写明验证方式。
9. 测试、性能验证和文档跟随所属任务完成，不集中拖到最后。
10. 任何涉及运行时资源、环境变量或部署参数的方案，必须单独安排配置验证任务：定义 schema 和环境示例、覆盖关键绑定缺失的负向测试、执行敏感信息扫描，并明确真实资源尚未创建时不能用占位值冒充通过。
11. Design 必须确认动态核心链路、数据来源与更新责任、平台能力与环境差异、可移植端口/适配器，以及测试门禁与生产上线门禁；环境能力不一致时拆分阶段验证任务，不得把本地替代验证写成生产平台已通过。

### 实施门禁 B

- design 没有引入 spec 之外的新功能；
- 会改变 spec、架构或任务拆分的问题已经关闭，不能放入可延期 Open Questions；
- tasks 覆盖所有 requirements/scenarios，首组任务形成可运行垂直切片；
- 持续更新不依赖 FDE 手工改代码或 Git；
- 新项目的技术选型有候选比较、明确结论、取舍和重审条件；
- 正常链路、异常链路、数据所有权和部署视图互相一致；
- 性能目标有容量假设和验证任务，可扩展性有具体边界与升级阈值；
- FDE/技术负责人一次性批准 `design + tasks`。
- Design 与 tasks 还必须明确动态核心链路、数据来源与长期更新责任、各环境能力矩阵、端口/适配器替换边界、测试环境证据和生产上线门禁；未验证的平台能力不得被本地替代测试掩盖。

通过前不得编码。旧项目的 `plan.md` 可以映射为 `design.md`，但不要同时维护两份。

## 阶段 3：交付

1. AI 按 tasks 逐项实现、验证并勾选，不自行扩大范围。勾选不是进度声明：只有约定验证已经实际执行，并记录执行环境、时间、结果和证据位置后才能改为完成；部分通过仍保持进行中，后续变更使证据失效时必须重新打开。
2. 发现需求错误回到阶段 1；发现实现路径错误回到阶段 2。
3. 全部场景通过后让客户操作真实预览或可运行仓库完成验收。
4. 只把已经实现并验收的 delta specs 合入 `openspec/specs/`，然后 archive change。
5. 未完成行为创建后续 change，不得提前写成当前系统事实。

每项完成证据至少回答：验证了哪一层、在哪里执行、实际结果是什么、谁复核、什么变化会使证据失效。单元测试替身只能证明局部业务规则；真实数据库、异步任务、API、目标运行时和公开入口分别需要对应层级的证据，不能互相替代。

## 新人的第一条指令

```text
读取客户确认内容、目标 Demo、当前仓库和 openspec/specs。
本轮只执行“定义阶段”，不要讨论技术实现，不要修改代码。
建立一个聚焦的 change，生成 proposal 和不超过 3 个 capability specs。
每项 requirement 至少包含一个 WHEN/THEN scenario。
完成范围、一致性、边界和可验收性检查后停下，等待需求门禁 A。
```

## 最少命令

```text
# 首次安装和初始化
npm install -g @fission-ai/openspec@latest
openspec init

# 日常只需检查这两项
openspec status --change <change-id>
openspec validate <change-id> --strict
```

AI 对话命令因 Codex、Cursor 等工具而异，以 `openspec init/update` 实际安装的命令为准。

## 失败处理

- change 不能用一句话描述：拆分，不继续加文档。
- capability 超过 3 个：先尝试按用户结果合并；确实能独立验收或演进时才保留。
- spec 出现技术名词：移到 design。
- requirement 没有 scenario：不能过门禁 A。
- design 引入新行为：退回阶段 1。
- tasks 没有验证方式：不能过门禁 B。
- 实现与 spec 不同：修订并重新确认，不允许只改代码。

## 本轮验证指标

- 新人能否只看本页说出三个阶段和下一步；
- 需求返工次数；
- tasks 可独立完成并验证的比例；
- 客户问题能否追溯到 requirement/scenario；
- 交付后当前 specs 是否与真实系统一致。
