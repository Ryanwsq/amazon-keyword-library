---
name: amazon-keyword-trend-analysis
description: Build one category-level monthly and quarterly trend sheet for classified Amazon Sheet2 F1-F3 keywords using at least 24 complete months of exact SellerSprite search volume. Use for月度环同比、季度环同比、趋势矩阵和折线图；do not use for competition, source mining, SIF trends, seasonality labels or ad decisions.
---

# Amazon Keyword Trend Analysis

## 目标

只用卖家精灵精确词月搜索量，为Sheet2 F1–F3生成至少24完整月证据、最近12月与4季度矩阵及两张环比/同比折线图。

## 输入

锁定分类工作簿、Sheet2 F1–F3人口、Keyword_ID、站点、最新完整月、查询批次、趋势版本和获准卖家精灵趋势接口。

## 输出

只含`Sheet8_品类关键词趋势性`的过程工作簿、trend manifest和紧凑状态；不把24个月逐词数据贴进主任务对话。

## 可调用能力

- `keyword.library.trend.analyze`
- `keyword.trend.sellersprite.query`
- `keyword.trend.outputs.write-and-verify`

## 执行步骤

1. 读取知识、判断边界和`references/output-contract.md`，锁定F1–F3人口、主键、站点、最新完整月、24月范围和版本。
2. 对每个完整英文关键词执行卖家精灵精确词趋势查询；不用词根、近义词、SIF或其他来源替代。
3. 保存实际返回和月份；当前未结束月排除。月份为空/不可解析无效，缺值留空，不填0、不插值。
4. 至少形成24个完整月查询范围；最近12完整月进入月度矩阵，额外历史用于同比和季度基准。
5. 计算月环比/月同比。当前或基准缺失、基准为0时留空。
6. 按完整自然季度求和；任一月缺失则该季度搜索量、环比、同比全部留空。展示最近4完整季度。
7. 写入关键词索引、36行月度矩阵、12行季度矩阵和两张百分比折线图。每个关键词两条线，同词同色，环比实线、同比虚线。
8. 验证人口、月份、季度、空值、公式、矩阵列、图表范围和序列数；渲染Sheet及两图。

## 质量标准

- 人口恰好等于Sheet2 F1–F3，其他人口零混入。
- 每词使用卖家精灵精确词且至少查询24个完整月。
- 月度固定12×3，季度固定4×3，每词在两矩阵恰好一列。
- 两图各有`关键词数×2`理论序列且不混入搜索量。
- 无趋势标签、季节性、广告资格或行动建议。
- Skill保持draft/planned，未完成真实三案例不称verified。

## 异常处理

接口不可用或全局无完整月为`not_executed`；至少一词零有效数据或环同比基准整体不足为`incomplete`；各词有数据但部分月份缺失可为`completed_with_gaps`；主键、人口、矩阵或图表无法闭合为`blocked`。不回退其他来源。
