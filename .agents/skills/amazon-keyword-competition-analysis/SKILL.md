---
name: amazon-keyword-competition-analysis
description: Build the standalone competition-analysis sheet for classified Amazon Sheet2 F1-F4 keywords using only exact SIF Top3 click and conversion shares. Use for关键词头部锁定竞争等级、Top3结构分化和竞争输出质检；do not use for trend charts, source mining, classification, negative targeting or ad eligibility.
---

# Amazon Keyword Competition Analysis

## 目标

只为 Sheet2 F1–F4 关键词建立独立、可复算的竞争性分析 Sheet，并只用SIF Top3点击份额与Top3转化份额输出综合竞争等级。

## 输入

锁定的第二板块及分类版本、Sheet2 F1–F4、稳定 `Keyword_ID`、站点、分析周期、第一板块SIF最小字段表，以及获准的SIF关键词历史接口。

## 输出

一词一行的`第三板块_关键词竞争性分析`Sheet、竞争记录 ID、Top3两项正式输入、绝对层、结构分化、综合等级、摘要、完整性、置信度和人工复核状态；只向最终装配回传五个竞争引用/摘要字段。

## 可调用能力

- `keyword.library.competition.analyze`
- `keyword.competition.sif-top3.query`
- `keyword.competition.outputs.write-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/output-contract.md`，锁定人口、版本、站点、周期和停止门；不读取 SKU 事实卡。
2. 只选择 Sheet2 F1–F4，并为每个 `Keyword_ID` 建立唯一 `Competition_Record_ID`；F5、Sheet3、Sheet4 范围外。
3. 先按完整关键词读取锁定第一板块SIF表中的Top3点击/转化份额、来源竞品和周期。同词多竞品记录一致时合并来源；不同值不平均、不选择性覆盖。
4. 对Top3任一缺失或同周期冲突的完整关键词，使用`keyword.competition.sif-top3.query`调用SIF `market_get_keyword_history`，每批最多10词、固定`granularity=week`，保存完整响应后只读取latest对象中的date、top3_click_share和top3_conversion_share。不得读取历史均值作当前值，不得用词根、近义词或相关词替代。
5. 按合同分别计算Top3点击绝对层、Top3转化绝对层和差值结构，再把头部锁定度直接写为综合竞争等级。当前没有比较池、样本门、相对分位、CPC、SPR、商品数量、市场CVR、入场信号或辅助格局修正。
6. 使用 `keyword.competition.outputs.write-and-verify` 写入独立竞争 Sheet，核对人口、主键、来源周期、完整词状态、Top3完整性、派生等级、缺失/冲突阻断和最终写回字段。

## 质量标准

- 竞争 Sheet 只包含 F1–F4且一词一行。
- 正式 SIF 数值只包含完整词Top3点击与Top3转化；优先复用第一板块，必要时批量精确补查。
- 综合等级只等于Top3两项合成的头部锁定度，不受其他字段或样本数影响。
- Top3任一缺失、周期不可追溯或冲突未解决时综合等级为空并标记`数据不足/人工复核`。
- 最终 Sheet1 只接收竞争记录ID、等级、摘要、完整性和复核状态。
- 不生成趋势、SKU匹配、广告资格或投放动作；Skill保持`draft`。

## 异常处理

Top3点击或转化缺失、完整词无精确记录、同词同周期冲突未解决、周期不可追溯或非F1–F4时，不输出正式综合等级。保留原值和来源，标记`数据不足/人工复核`并回传主任务。
