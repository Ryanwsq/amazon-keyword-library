# Amazon Keyword Library guidance

先读 `PROJECT.md`、`docs/thread-architecture.md`、`docs/task-routing.md`、`docs/risk-gates.md`、`docs/end-to-end-workflow.md`、`knowledge/INDEX.md`、`knowledge/product-keyword-library.md`、`knowledge/keyword-decision-log.md` 和 `docs/keyword-judgment-boundaries.md`，再按逻辑角色完整读取对应项目 Skill 及其直接引用资料。

## Content ownership

- 稳定领域知识归 `knowledge/product-keyword-library.md`。
- 历史案例证据归 `knowledge/keyword-cleaning-case-evidence.md`；已确认版本决策归 `knowledge/keyword-decision-log.md`。
- 当前完成度、开放问题和下一步归 `PROJECT.md`。
- 从用户输入到本机输出的人类可读流程归 `docs/end-to-end-workflow.md`；它只汇总拥有文件中的当前规则，不成为第二套规则来源。
- 相关性、停止条件、人工升级和发布门槛归 `docs/keyword-judgment-boundaries.md`。
- 可重复执行步骤归 `.agents/skills/<single-purpose>/`。
- 不在 Skill 中复制整篇知识库，不在知识文章中维护逐步执行 SOP。

## Mandatory workflow synchronization

任何对本项目 `knowledge/` 或 `.agents/skills/` 的新增、修改、重命名、废弃或状态调整，都必须在同一批变更中更新 `docs/end-to-end-workflow.md` 的同步日期、同步说明和受影响章节。即使端到端流程没有变化，也必须在同步说明中明确记录“已复核，无流程变化”。未同步该文档时，知识或 Skill 变更不得通过项目验收。

业务规则必须先修改其拥有文件，再同步到端到端说明；不得只在说明文档中修改阈值或执行规则。

## Codex discovery

所有项目任务都从本独立仓库根目录或其子目录启动。本仓库的当前长期主任务标题固定为 `Amazon关键词词库｜主任务｜main`；长期副任务标题、所有权和执行依赖由 `docs/thread-architecture.md` 与 `docs/thread-roles.md` 负责。

实际 Codex 任务 ID、绝对路径、Worktree 和本机授权状态只能写入被 Git 忽略的 `thread-map.local.md`，不得进入任何可同步文件。

## Task separation

主任务负责输入锁定、运行版本、长期副任务调度、三来源机械合并、阶段完成门、跨模块冲突和最终验收。每个长期副任务独立负责一项执行任务，以及该模块对应的 Skill、直接引用合同和模块知识；副任务之间不直接交接正式结论，统一回传主任务。

正式只读验证期间，所有 Skills、知识和流程文件保持只读；业务文件只写入 `.local/runs/<Run_ID>/`。发现的问题只进入候选问题台账，不得边跑边改规则。用户确认后，副任务才可进入获准迭代模式修改自己拥有的文件，主任务统一同步项目决策、知识索引和端到端流程。

Amazon联想是必选来源，固定使用Codex内置浏览器、Amazon US未登录、All、10001；不可操作时必须`not_executed`且正常案例未完成。第二板块按动态完整商品对象分流，不按SKU配置删除同品类词；拼写错误、其他语言和自有品牌不得一律摘除。分类F5按唯一主标签分组且每词只出现一次。竞争机会筛选无精确值或回退样本不足10时不出综合等级；趋势月份为空时不生成图。P1验收未完成时不得冒充已完成或已验证能力。

## Status integrity

当前唯一清洗基线仍是V2.1；V2.2只标识词频组件。2026-08-20回写的是首轮正常案例规则修正，不命名为新清洗版本。旧工作簿在规则修正前生成，只能作为旧运行证据；补齐联想并按新规则重跑、输出去向差异后才能继续验收。Sheet3否词边界、广告资格、广告框架和品类通用词库仍待定。来源采集已拆为SIF、Amazon联想和卖家精灵三个单一职责Skill，竞争与趋势已拆分，并建立独立质量验证Skill；当前共十二个Skills，全部保持`draft`且能力为`planned`，本次结构拆分和P0通过不生成P1证据。

## Data boundary

原始聊天、原始 XLSX、截图、检查日志、源电脑路径、任务 ID、账号信息和联系信息只可作为获准的本地证据，不进入仓库。归属、公司政策、仓库所有权或共享范围不确定时停止并向用户升级。

## GitHub synchronization checkpoints

- 每完成一个已确认项目阶段，必须把稳定知识、判定边界、Skills、项目状态、人类文档和必要交接同步到获准的 GitHub 工作台，并报告分支、提交和远端结果。
- 准备从家庭电脑切换到工作电脑，或从工作电脑切回家庭电脑前，必须先更新交接、运行仓库验证并完成 GitHub 同步；另一台电脑从远端基线继续，不依赖本机聊天或未提交文件。
- 原始 ZIP、聊天、XLSX、截图、运行日志、任务 ID、绝对路径和本机插件状态不因阶段同步进入 Git。
- 每次同步仍遵守短期分支、显式暂存、验证、非强制推送和远端回读规则；同步检查未完成时，不把该阶段标记为可跨电脑继续。
