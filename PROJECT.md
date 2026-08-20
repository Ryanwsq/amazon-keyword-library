# Amazon Keyword Library
- Project name: Amazon产品关键词知识库
- Project slug: `amazon-keyword-library`
- Purpose: 采集、语义分类、清洗、去重并维护跨类目可复用的关键词知识。
- Primary users: Amazon运营、Listing和广告团队
- Repository sharing classification: sanitized
- Last reviewed: 2026-08-20

## Current state

- Repository split status: the standalone project is connected to the private GitHub repository `Ryanwsq/amazon-keyword-library`; `main` is the only long-lived baseline branch, and the initial sanitized repository revision has been published.
- Main task: the current Codex task is assigned the logical role `keyword-main` with title `Amazon关键词词库｜主任务｜main`.
- Task model: one persistent main task plus persistent single-responsibility side tasks. Side tasks are not recreated for each run and are not bound to permanent Git branches.
- Persistent side tasks: all ten logical side tasks have been created in the standalone Codex project on this device, completed read-only initialization and are idle awaiting a locked `Run_ID`; actual IDs, paths and authorization state remain only in the ignored local thread map.
- Parallel schedule: after SIF and main-task anchor/seed confirmation, Amazon autocomplete and SellerSprite expansion run in parallel; after cleaning closure, word frequency and classification run in parallel; after classification, competition and trend run in parallel. Merge and final assembly wait for all applicable branches.
- First formal run in this repository will be a fixed-baseline read-only validation using a new real case supplied by the user. The current gaming-chair run remains migration-era issue-discovery evidence and is not that formal validation.
- Current knowledge baseline: V2.1.
- Collection framework is confirmed; category cleaning has four historical cases and a V2.1 layer correction.
- The third-board and final-workbook structure is confirmed: classify core, secondary and excluded terms; analyze competition for Sheet2 F1–F4 in a separate sheet; create one category F1–F3 historical monthly search-volume line-chart sheet; then assemble a Sheet2-only final decision master plus all process sheets. A classification draft Skill encodes the confirmed structure with manual stops for unresolved boundaries; competition boundaries, the trend-chart form and its SellerSprite source are confirmed; the negative-keyword boundary remains incomplete.
- The V2.2 word-frequency component has been imported from the user-provided sanitized cross-device package. It is a second-board mechanical step that reads only Sheet2, outputs ranked single-word and adjacent ordered two-word occurrence counts in `Sheet5_词频统计`, and is preserved as a process Sheet; it is not a classification, traffic or advertising signal.
- The current baseline contains ten business Skills—SIF competitor collection, Amazon autocomplete, SellerSprite expansion, category cleaning, word frequency, keyword classification, competition analysis, trend analysis, final-workbook assembly and independent quality validation—and two maintenance Skills for main-task operations and repository publication. All twelve remain `draft`; all registered capabilities remain `planned`. The two combined migrated packages have been replaced by single-responsibility packages without changing confirmed business rules, and the current repository P0 passes for all twelve packages.

| 板块 | 状态 | 已确认内容 | 未完成内容 |
|---|---|---|---|
| 01 关键词收集 | 规则已回写，三个单一职责draft Skill已建立；正常案例进行中 | SIF即时落盘；必选联想固定内置浏览器/未登录/All/10001；卖家精灵分页漂移/重复；规范ABA/搜索量来源；主任务机械合并 | 内置浏览器可打开但不能识别/操作，联想`not_executed`；三个Skill均无P1 |
| 02 关键词清洗与词频 | V2.1清洗、V2.2词频和首轮边界修正已确认 | 动态中心对象；自有品牌、拼写、其他语言、类目等价表达；Sheet2/3/4唯一去向；词频只读Sheet2 | 新规则尚未重跑；Sheet3否词边界继续验证；两个Skill无P1 |
| 03A 关键词分类 | 结构与F5分组已确认，draft Skill已更新 | F1–F5、多标签；F5唯一主分组标签、标签内每20词拆LT且每词只出现一次；Sheet4独立；Sheet3候选 | 新规则尚未重跑；否词边界无P1 |
| 03B 竞争性与趋势性 | 两个单一职责draft Skill已建立；来源、严格样本门和月份门已确认 | F1–F4独立竞争；机会筛选精确值正式、竞争格局辅助；样本不足10不出等级；F1–F3卖家精灵月度折线图且月份非空 | 新规则尚未重跑；两个Skill均无P1；趋势不可回退其他来源 |
| 03C 最终工作簿 | Sheet人口和保留规则已确认，draft Skill 已更新 | 第一张总表只收录 Sheet2 核心词；竞争、Sheet3、Sheet4、词频、趋势各自独立；竞争只回写记录ID、综合等级、摘要、完整性和复核状态；三个板块全部过程 Sheet 保留 | 缺少应有的词频 Sheet 时回到词频 Skill 补齐，不在装配阶段重算；分类异常和否词最终规则完成前只能装配已确认输入；新 Skill 尚无 P1 |
| 独立质量验证 | draft Skill已建立，P0通过 | 只读核对Run/revision、来源、主键、人口、版本、公式、渲染、Sheet和图表闭环；输出pass/blocked/incomplete | 尚未执行独立仓库真实案例P1；不修复上游、不调用业务外部系统 |
| 发布与版本 | 阶段性可维护 | 已确认内容与待确认内容分层，版本可追踪 | 尚未形成经三案例验证的发布 Skill |

## Open questions

- Codex 内置浏览器可以拉起但不能识别/操作 Amazon 页面的技术问题怎样解决；解决前联想保持`not_executed`。
- Sheet3 中词组否定、精准否定、不否定和人工确认的判定边界，以及如何防止词组否定误伤 Sheet2/Sheet4。
- 最终关键词决策总表之后的广告资格、广告框架和单 Sheet 品类通用词库均待后续单独确认，不属于当前第三板块第一、二部分。
- 首轮正常案例补齐联想后，怎样按2026-08-20新规则重跑并输出旧去向到新去向差异；其后再完成第二个正常案例和边界/异常案例。
## Success criteria
- 每个词保留来源、类目语义、意图、状态和版本。
- 分类规则跨类目稳定，类目专属词不被错误泛化。
- 清洗闭环可复核，不把待确认研究写成已验证规则。
## Main outputs
- 当前可输出关键词知识基线、判定边界和各工作簿合同。首轮旧规则工作簿仅为本机验证证据；在联想补齐和新规则重跑前，不作为当前正式品类词库交付或P1证据。否词最终判断仍需补齐边界。

## Source of truth map

- 人类可读端到端流程：`docs/end-to-end-workflow.md`
- 稳定领域知识：`knowledge/product-keyword-library.md`
- 历史案例证据：`knowledge/keyword-cleaning-case-evidence.md`
- 已确认版本决策：`knowledge/keyword-decision-log.md`
- 当前完成度、未完成项和下一步：本文
- 判定门槛与人工升级：`docs/keyword-judgment-boundaries.md`
- 可重复执行流程：`.agents/skills/<single-purpose>/`
## Boundaries
- 不保存账号、店铺授权、原始广告导出或未脱敏业务数据。
- 稳定知识、判定边界和执行流程分别维护，不把历史案例反向写成新 Skill 的验证证据。
- 修改任何项目知识或 Skill 时，同批同步 `docs/end-to-end-workflow.md`；未同步不能验收。
