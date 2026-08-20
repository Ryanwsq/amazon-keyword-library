---
name: amazon-keyword-competition-analysis
description: Build the standalone competition-analysis sheet for classified Amazon Sheet2 F1-F4 keywords using exact SIF opportunity values and locked SellerSprite fields. Use for关键词竞争等级、样本门和竞争输出质检；do not use for trend charts, source mining, classification, negative targeting or ad eligibility.
---

# Amazon Keyword Competition Analysis

## 目标

只为 Sheet2 F1–F4 关键词建立独立、可复算的竞争性分析 Sheet，并按已确认完整性和样本门输出综合竞争等级。

## 输入

锁定的第二板块及分类版本、Sheet2 F1–F4、稳定 `Keyword_ID`、站点、分析周期、第一板块 `06_卖家精灵原始数据`批次，以及获准的两个 SIF 竞争接口。

## 输出

一词一行的`第三板块_关键词竞争性分析`Sheet、竞争记录 ID、正式输入、辅助证据、比较池、派生等级、综合等级、摘要、完整性、置信度和人工复核状态；只向最终装配回传五个竞争引用/摘要字段。

## 可调用能力

- `keyword.library.competition.analyze`
- `keyword.competition.sif-landscape.query`
- `keyword.competition.sif-opportunity.screen`
- `keyword.competition.outputs.write-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/output-contract.md`，锁定人口、版本、站点、周期和停止门；不读取 SKU 事实卡。
2. 只选择 Sheet2 F1–F4，并为每个 `Keyword_ID` 建立唯一 `Competition_Record_ID`；F5、Sheet3、Sheet4 范围外。
3. 通过稳定主键只读第一板块卖家精灵批次中的 SPR 和商品数量。不得重新调用关键词挖掘或局部补拉；卖家精灵 CPC、集中度仅作溯源。
4. 使用 `keyword.competition.sif-opportunity.screen` 取得完整关键词精确匹配的 CPC、Top3点击/转化份额、市场CVR、入场信号、Top3 ASIN和周期；这些值是正式判断来源。无精确匹配时不得以词根、近义词或相关词替代。
5. 按需使用 `keyword.competition.sif-landscape.query` 补充集中度历史、结构分化、Top3外购买空间、头部流量结构和可进入性解释；当前不传目标 ASIN，不使用目标位置和广告建议。两个接口同名值不平均、不覆盖。
6. 按合同计算 Top3绝对层、点击转化差、CPC/SPR/商品数量/CVR相对层和有序综合等级。比较池按同层、相邻层组合、全部F1–F4回退；仍少于10个有效词时不输出相对层或综合等级。
7. SIF入场信号只作一致性复核，不重复计分；两级以上未解决冲突阻断正式等级。
8. 使用 `keyword.competition.outputs.write-and-verify` 写入独立竞争 Sheet，核对人口、主键、来源周期、精确匹配、比较池、样本数、完整性和最终写回字段。

## 质量标准

- 竞争 Sheet 只包含 F1–F4且一词一行。
- 正式 SIF 数值来自机会筛选完整词精确匹配，竞争格局只作辅助。
- SPR和商品数量来自第一板块锁定批次，没有重采或补拉。
- 回退后有效样本不足10时综合等级为空并标记`数据不足/人工复核`。
- 最终 Sheet1 只接收竞争记录ID、等级、摘要、完整性和复核状态。
- 不生成趋势、SKU匹配、广告资格或投放动作；Skill保持`draft`。

## 异常处理

Top3点击或转化缺失、CPC/SPR/商品数量缺失两项以上、机会筛选无精确匹配、回退后样本不足10、周期方向冲突、入场信号两级以上冲突或非F1–F4时，不输出正式综合等级。保留原值和可独立判断证据，标记`数据不足/人工复核`并回传主任务。
