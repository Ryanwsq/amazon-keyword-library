# Amazon Keyword Library

- Project name: Amazon产品关键词知识库
- Project slug: `amazon-keyword-library`
- Purpose: 从真实产品输入开始，完成关键词来源采集、品类清洗、分类、词频、竞争、趋势、最终装配和独立验收。
- Primary users: Amazon运营、Listing和广告团队
- Repository sharing classification: sanitized
- Last reviewed: 2026-08-21

## Current state

- 独立私有仓库：`Ryanwsq/amazon-keyword-library`；`main`是唯一长期基线，当前合同更新在短期功能分支完成。
- 当前长期主任务：`Amazon关键词词库｜主任务｜main`；十个长期单一职责副任务已在本机建立，实际任务ID、路径和授权只保存在Git忽略映射。
- 当前知识基线仍是清洗V2.1；V2.2只标识词频组件。2026-08-21的来源减配、输出合同和装配规则属于Post-V2.1增量，不命名为新清洗版本。
- 当前共十二个Skills：十个业务Skill和两个维护Skill。全部保持`draft`，全部能力保持`planned`；本轮文档、合同和P0验证不构成P1。
- 首轮真实正常案例已经跑到旧结构的最终工作簿和独立QA，但Amazon联想为`not_executed`，且结果生成在本次输出合同锁定之前，只能作为问题发现证据。
- 正式运行固定为一次用户输入后由主任务调度至最终交付；运行期间知识、Skills和流程只读，问题统一进入一个问题文档，整轮结束后再修改规则。

## Confirmed execution graph

1. 主任务锁定产品事实、类目锚点候选、竞品、站点、Run和规则版本。
2. SIF竞品反查先运行；主任务确认一个主核心大词和卖家精灵代表种子。
3. Amazon联想与卖家精灵扩词并行。
4. 主任务机械合并三来源，输出第一板块两Sheet工作簿。
5. 第二板块完成Sheet2/3/4三去向清洗。
6. 清洗闭环后，词频与关键词分类并行。
7. 分类完成后，竞争性与趋势性并行。
8. 最终装配生成过程文件夹和七Sheet最终工作簿。
9. 独立质量验证只读执行21项装配门；未通过不得标记完成。

## Module status

| 模块 | 当前合同 | 当前状态 |
|---|---|---|
| SIF竞品反查 | 七列业务明细和紧凑核心词候选；完整响应仅本地证据 | draft/planned；旧首轮证据不可作为P1 |
| Amazon联想 | 必选；内置浏览器、Amazon US未登录、All、10001、完整固定矩阵 | draft/planned；适配问题仍待解决 |
| 卖家精灵扩词 | 四列业务表；有界最大召回与损失风险 | draft/planned |
| 三来源机械合并 | 两Sheet：关键词池、任务信息与类目锚点 | 合同已确认，待新Run重跑 |
| 品类清洗 | 四Sheet：事实与锚点、品类相关、其他摘除、二类词 | V2.1边界不变；字段合同已减负 |
| 分类 | Sheet2追加4固定列+N动态语义列；Sheet4追加4固定列；最小否词库 | 合同已确认，待P1 |
| 词频 | Sheet2英文词；去除介词且介词为双词断点；两张三列表 | V2.2组件规则已更新，待P1 |
| 竞争 | Sheet2 F1–F4；只用SIF Top3点击/转化份额；12列 | 合同已确认，待P1 |
| 趋势 | Sheet2 F1–F3；卖家精灵至少24完整月；月/季环比同比及两图 | 合同已确认，待P1 |
| 最终装配 | 两个顶层对象；最终工作簿固定七Sheet；总表覆盖三去向50+N列 | 合同已确认，待P1 |
| 独立质量验证 | 两Sheet质量报告、唯一问题文档、21项装配门 | draft/planned，待新Run验证 |

## Final delivery contract

顶层只交付：

1. `过程性文件/`：分第一板块、第二板块、第三板块和独立质量验证四个目录，并含唯一`process-manifest.json`。
2. `<Run_ID>-最终关键词词库.xlsx`：仅七个可见Sheet，顺序固定为`最终关键词决策总表`、`SKU事实卡`、`品类产品通用词库`、`关键词竞争性分析`、`关键词趋势性分析`、`词频统计`、`否词库`。

最终总表覆盖第一板块机械去重全人口，每个`Keyword_ID`恰好一行并标记`品类相关/其他摘除/二类词`。详细Top3及竞争结构只在独立竞争Sheet；广告资格和投放动作当前统一为`未评估/后置`。

## Open work

- 修复或替换内置浏览器无法识别Amazon联想页面的适配问题；联想未执行时正常案例仍不完整。
- 从锁定仓库revision启动新的正式只读Run，按当前合同完整重跑该真实案例，并只把错误记录到唯一问题文档。
- 完成两个正常案例和一个边界/异常案例的真实P1；此前不得提升Skill成熟度。
- 广告否定方式、广告资格和投放动作仍是后置模块；当前否词库仅保留语义否词候选，不输出精准/词组否定类型。

## Success criteria

- 三来源尽可能完整抓取；局部字段或接口异常不吞掉已经取得的关键词。
- 第一板块机械词池、第二板块三去向和最终总表人口逐层闭合，主键、来源、周期和版本可追溯。
- 分类、词频、竞争、趋势和通用词库均可从锁定上游机械复算。
- 最终只有两个顶层交付对象，最终工作簿恰好七个可见Sheet。
- 独立QA通过全部适用硬门后才可标记`completed`或`completed_with_gaps`。

## Source of truth map

- 人类可读端到端流程：`docs/end-to-end-workflow.md`
- 稳定领域知识：`knowledge/product-keyword-library.md`
- 历史案例证据：`knowledge/keyword-cleaning-case-evidence.md`
- 已确认版本决策：`knowledge/keyword-decision-log.md`
- 当前完成度和下一步：本文
- 判定门槛与人工升级：`docs/keyword-judgment-boundaries.md`
- 可重复执行流程：`.agents/skills/<single-purpose>/`

## Boundaries

- 不保存账号、Token、原始接口响应、真实业务工作簿、任务ID、绝对路径或未脱敏数据到Git或飞书发布正文。
- 修改任何项目知识或Skill时，同批同步`docs/end-to-end-workflow.md`；未同步不得验收。
- 正式只读运行中不改规则；问题先集中记录，运行结束并经用户确认后才进入规则迭代。
