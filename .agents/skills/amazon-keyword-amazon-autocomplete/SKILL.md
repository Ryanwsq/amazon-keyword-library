---
name: amazon-keyword-amazon-autocomplete
description: Capture the required Amazon US search-box autocomplete matrix in the Codex built-in browser. Use for第一板块Amazon联想、固定未登录环境和可见建议证据；do not use for search results, web search, SIF, SellerSprite, source merging or semantic filtering.
---

# Amazon Keyword Amazon Autocomplete

## 目标

在固定可复核环境中，只围绕主一级品类核心大词采集必选 Amazon 搜索框可见联想矩阵。

## 输入

锁定的 `Run_ID`、唯一主锚点、站点 US、真实品类功能/配置探针、规格敏感性判断、本机忽略输出目录和 Codex 内置浏览器。

## 输出

触发输入清单、每个输入的成功/无建议/失败状态、全部可见建议与顺序、实际浏览器环境、登录状态、Department、邮编、时间、执行次数和证据指针。

## 可调用能力

- `keyword.source.autocomplete.capture`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/source-contract.md`，核对主锚点、探针、固定环境和停止门。
2. 只使用 Codex 内置浏览器打开 Amazon US，确认未登录、Department=`All`、配送邮编=`10001`；记录真实环境，不写无痕模式。
3. 依次执行基础输入、后置 A–Z、前置 A–Z、`for/with/without`、真实品类功能/配置的前后组合；规格敏感产品再执行0–9，否则记录不适用。
4. 每个输入等待下拉建议稳定，只读取当次可见建议和顺序；不按 Enter、不进入结果页、不递归扩展新词、不使用`how/what/why`。
5. 功能/配置探针记录目标 SKU 为具备、不具备或待核实；联想结果不能转成产品宣称。
6. 每个输入保存状态、建议、环境、时间和可见证据指针；同一建议由多个输入触发时保留全部来源事件。
7. 核对适用矩阵输入均有状态，形成只含联想来源的回传清单；不合并到总词池。

## 质量标准

- 环境固定且记录完整，没有外部浏览器或搜索结果页替代。
- 每个适用输入都有成功、无建议或失败状态。
- 只保存可见下拉建议，重复触发来源不丢失。
- 建议顺序只作为原始字段，不被解释为流量等级。
- 不执行来源合并、语义判断或产品宣称。

## 异常处理

页面能打开但不能稳定识别或操作时，保存未执行证据并回传`not_executed/incomplete`；不尝试其他浏览器、搜索结果页、网页搜索或 API。页面状态、登录状态、部门或邮编无法确认时同样停止，不能声称矩阵完成。
