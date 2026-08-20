---
name: amazon-keyword-trend-analysis
description: Build one category-level historical monthly search-volume line chart for classified Amazon Sheet2 F1-F3 keywords using SellerSprite only. Use for品类关键词历年月度趋势矩阵、折线图和月份质检；do not use for competition scoring, source mining, SIF trends, trend labels or ad decisions.
---

# Amazon Keyword Trend Analysis

## 目标

只用卖家精灵精确词月度搜索量，为一个品类的 Sheet2 F1–F3 关键词生成一个可复算趋势 Sheet和一张多序列折线图。

## 输入

锁定的第二板块及分类版本、Sheet2 F1–F3、稳定 `Keyword_ID`、站点、查询批次、最早可用月份、最新完整月份和获准的卖家精灵 `aba_research_trend`。

## 输出

一个品类趋势性分析 Sheet：元数据、`YYYY-MM`月度搜索量矩阵、一张每词一条线的折线图、缺失月份和完整性状态；不写回最终 Sheet1。

## 可调用能力

- `keyword.library.trend.analyze`
- `keyword.trend.sellersprite-aba.query`
- `keyword.trend.output.write-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/output-contract.md`，锁定人口、站点、月份和停止门；不读取 SKU 事实卡。
2. 只选择 Sheet2 F1–F3；F4、F5、Sheet3、Sheet4范围外。
3. 对每个完整关键词使用 `keyword.trend.sellersprite-aba.query` 获取全部可用历史月度搜索量。每条记录的月份必须非空且可解析为`YYYY-MM`；当前未结束月份不进入。
4. 空月份响应无效，只能在获准参数范围内调整后重查；不得自行生成月份。卖家精灵不可用或无可用完整月时标记`not_executed`，不调用 SIF 或其他平台替代。
5. 统一站点、月份范围和搜索量口径，在一个 Sheet 中建立矩阵：第一列月份，后续每列一个关键词；缺失月份留空并记录，不填0、不插值、不前值延续。
6. 使用 `keyword.trend.output.write-and-verify` 生成一张折线图：横轴年-月，纵轴月度搜索量，每个关键词恰好一条线，图例为关键词。
7. 核对 F1–F3人口、关键词列、月份升序、非空月份、图表序列、图例、来源和周期；趋势不写回最终总表。

## 质量标准

- 一个品类只有一个趋势 Sheet和一张折线图。
- 仅包含 Sheet2 F1–F3，每个主键恰好一条图表序列。
- 正式来源仅卖家精灵月度搜索量，月份非空可解析。
- 缺失月份留空且可识别，没有填0、插值或跨来源补值。
- 不生成近三月、同比、ABA走势、峰值、季节性、汇总线、趋势标签或广告动作。
- Skill保持`draft`，没有真实三案例前不声称已验证。

## 异常处理

卖家精灵趋势接口不可用、月份为空或无可用完整月时标记`not_executed`，不生成替代图。单词存在部分月份缺失时保留空值和覆盖范围；主键、人口、月份或序列无法闭合时阻断交付并回传主任务。
