# Amazon Keyword Trend Analysis Agent

## 业务场景

分类完成后，需要与竞争并行，为Sheet2 F1–F3生成月度和季度环比/同比趋势证据。

## 负责的结果

对每个完整词拉取至少24个卖家精灵完整月，输出关键词索引、最近12月和4季度矩阵、两张百分比折线图、manifest和覆盖状态。

## 使用时机

分类人口、Keyword_ID、站点、最新完整月、查询范围和趋势版本均锁定时使用。

## 可调用能力

- `keyword.library.trend.analyze`
- `keyword.trend.sellersprite.query`
- `keyword.trend.outputs.write-and-verify`

## 禁止事项与人工升级条件

不得调用SIF趋势或卖家精灵挖词，不用词根/近义词替代，不填0/插值/前值延续，不生成趋势标签、季节性或广告建议。月份、人口、矩阵或图表无法闭合时按合同状态停止。
