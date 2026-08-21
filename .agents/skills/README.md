# Project Skills

本项目把稳定知识、判定边界和执行流程分开维护：

- 人类可读端到端流程：`../../docs/end-to-end-workflow.md`
- 稳定领域知识：`../../knowledge/product-keyword-library.md`
- 历史案例证据：`../../knowledge/keyword-cleaning-case-evidence.md`
- 版本决策：`../../knowledge/keyword-decision-log.md`
- 当前状态与开放问题：`../../PROJECT.md`
- 判定边界：`../../docs/keyword-judgment-boundaries.md`
- 执行流程：下列单一职责 Skill 包

| Skill | Capability | Responsibility | Maturity |
|---|---|---|---|
| `amazon-keyword-library-operations` | `keyword.library.version.manage`、`keyword.source.merge-and-assemble` | 主任务编排、版本、三来源机械合并、第一板块十表总门与验收 | draft |
| `amazon-keyword-sif-competitor-collection` | `keyword.source.competitor-traffic.query`、`keyword.source.sif.persist-and-verify` | 逐竞品SIF反查、完整响应本机落盘、七列明细和锚点候选证据 | draft |
| `amazon-keyword-amazon-autocomplete` | `keyword.source.autocomplete.capture` | 固定内置浏览器环境的Amazon可见联想矩阵 | draft |
| `amazon-keyword-sellersprite-expansion` | `keyword.source.keyword-mining.query`、`keyword.source.sellersprite.paginate-and-verify` | 卖家精灵四字段、2–3个收敛Pass、四列表装配和损失风险 | draft |
| `amazon-keyword-category-cleaning` | `keyword.library.clean` | 第二板块一级品类相关性三去向清洗 | draft |
| `amazon-keyword-word-frequency` | `keyword.library.word-frequency` | 只用 Sheet2 统计单词和相邻有序双词重复次数，输出最后一张 `Sheet5_词频统计` | draft |
| `amazon-keyword-classification` | `keyword.library.classify` 及四项分类/输出子能力 | Sheet2 F1–F5/LT与基础多标签、Sheet4二类词流量分类、Sheet3否词候选结构 | draft |
| `amazon-keyword-competition-analysis` | `keyword.library.competition.analyze`、`keyword.competition.sif-top3.query`、`keyword.competition.outputs.write-and-verify` | 为Sheet2 F1–F4建立Top3-only独立竞争Sheet，按完整词、周期和完整性输出等级 | draft |
| `amazon-keyword-trend-analysis` | `keyword.library.trend.analyze` 及两个查询/输出子能力 | 只用卖家精灵月搜索量生成一个品类F1–F3历年月度折线图 | draft |
| `amazon-keyword-final-workbook-assembly` | `keyword.workbook.final.assemble`、`keyword.workbook.sheet-manifest.verify` | 以 Sheet2 核心关键词建立第一张最终决策总表，只回写竞争引用/摘要并完整保留三板块过程 Sheet | draft |
| `amazon-keyword-quality-validation` | `keyword.quality.validate` | 独立只读复核Run/revision、来源、主键、人口、版本、公式、渲染、Sheet和图表完成门 | draft |
| `amazon-keyword-library-publication` | `keyword.library.publish` | 经审查知识的脱敏项目内发布 | draft |

当前共有十个业务 Skill（SIF、Amazon联想、卖家精灵、品类清洗、词频、关键词分类、竞争、趋势、最终装配、独立质量验证）和两个项目维护 Skill（operations、publication）。词频只执行 V2.2 组件已确认的 Sheet2 机械计数合同；竞争和趋势严格分离；质量验证只复核证据和完成门。十二个 Skill 均为 `draft`，全部能力为`planned`，没有真实 P1 三案例前不得声称已验证。

2026-08-20 已完成以下结构调整，并通过当前仓库 P0：

- `amazon-keyword-source-collection` 拆为 SIF竞品反查、Amazon联想采集和卖家精灵扩词三个单一职责 Skill；三来源机械合并继续由主任务控制。
- `amazon-keyword-competition-trend-analysis` 拆为竞争性分析和趋势性分析两个单一职责 Skill。
- 新增独立质量验证 Skill，统一执行数量、主键、版本、公式、渲染和图表闭环检查。

旧组合包文件已移除，原包到新包的映射保留在 `docs/thread-roles.md` 和版本决策记录中。结构拆分未改变业务规则，也不把任何能力升级为`verified`。

现有十二个包都遵守仓库根目录 `docs/skill-package-standard.md`。P0只表示结构可审查；历史案例或迁入前本地试算发生在当前包建立之前，不能反向当作新 Skill 的 P1 evidence。每个 Skill 需重新完成两个正常案例和一个边界/异常案例，才可考虑升级为 `verified`。

任何项目知识或 Skill 变更都必须在同一批变更中同步 `../../docs/end-to-end-workflow.md`；没有流程影响时也要更新其同步说明。

本仓库全部 Skills 都位于根目录 `.agents/skills/`。不要复制本机全局 Skill，也不要把原始业务数据、完整聊天或本机路径写入 Skill。
