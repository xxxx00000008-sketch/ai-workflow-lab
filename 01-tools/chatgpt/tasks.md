# ChatGPT Scheduled Tasks

> 最近验证：2026-09-19

## 定位

Scheduled 用于提醒、周期性执行和状态监控。它把“每次手动发同一提示”变成有时间规则、运行记录和通知策略的任务。

## 两种模式

- **持续线程型**：需要沿用当前项目上下文时，在同一任务中继续。
- **独立运行型**：每次执行互不依赖，适合固定报表或跨项目检查。

## 好任务的组成

- 明确频率和时区。
- 指定数据源、允许的动作和输出格式。
- 定义什么变化值得通知。
- 无变化时保持安静，避免通知疲劳。
- 说明失败、权限不足或需要审批时怎么办。

## 不适合自动化

不可逆操作、含糊目标、需要持续主观判断、未经授权的外部写入，以及没有稳定数据源的任务。

## 来源

- [开始使用 ChatGPT Work](https://learn.chatgpt.com/zh-Hans/docs/get-started-with-work)，访问于 2026-09-19。
- [Scheduled tasks — ChatGPT Learn](https://learn.chatgpt.com/fr-FR/docs/automations)，访问于 2026-09-19。
