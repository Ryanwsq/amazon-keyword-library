---
name: amazon-keyword-sif-competitor-collection
description: Collect and persist SIF traffic-keyword responses for approved direct competitor ASINs. Use for第一板块SIF竞品反查、最近30天原始响应落盘和类目锚点候选证据；do not use for autocomplete, SellerSprite expansion, source merging, cleaning or competition scoring.
---

# Amazon Keyword SIF Competitor Collection

## 目标

逐个反查获准直接竞品的最近30天流量词，并在任何汇总前保存完整、可追溯的原始响应。

## 输入

锁定的 `Run_ID`、站点、产品事实、3–5个直接竞品 ASIN、每 ASIN 300条接口上限请求、本机忽略批次目录和当前获准的 SIF MCP。

## 输出

逐竞品原始响应文件、请求与周期元数据、来源记录清单、异常日志、返回行数及一级品类核心大词候选证据；正式主锚点由主任务确认。

## 可调用能力

- `keyword.source.competitor-traffic.query`
- `keyword.source.sif.persist-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/source-contract.md`，核对站点、竞品范围、输出目录和停止门。
2. 在第一次查询前确认 `.local/runs/<Run_ID>/keyword-sif-collector/` 已建立且允许写入；目录或 Run 版本不清时停止。
3. 对每个获准竞品 ASIN 单独使用 `keyword.source.competitor-traffic.query` 查询最近30天，按 V2.1 已确认口径请求最多300条；不叠加7天窗口。
4. 每次返回后立即使用 `keyword.source.sif.persist-and-verify` 保存完整原始响应、全部入参、ASIN、站点、抓取时间、数据截止日、返回行数和批次 ID，再做字段映射或汇总。接口未返回起始日时写`接口未返回`，不得倒推。
5. 保留每一行和每一个返回字段，不按流量、重复或相关性删除。重试形成新调用版本，不覆盖先前响应。
6. 输出竞品覆盖和流量表达候选，供主任务结合产品事实确认唯一主锚点；本 Skill 不采用最终锚点或代表种子。
7. 核对每个竞品的调用状态、原始响应指针、返回行数、接口上限、周期和异常，并按合同生成回传清单。

## 质量标准

- 每次 SIF 返回先持久化后解析，完整响应可回查。
- 每个竞品和来源行有稳定 ID、站点、周期、数据截止日、抓取时间和批次。
- 300条只描述适配器上限；没有宣称 Amazon 全量。
- 原始行和字段未因重复、低流量或主观相关性而删除。
- 输出只提供锚点候选证据，不越权确认类目或执行其他来源任务。

## 异常处理

无数据时检查站点、父子体和 ASIN 状态并记录失败；不得换用未授权 ASIN。接口不可用、响应结构冲突、原始响应无法保存或来源授权不清时立即停止剩余查询，保留已取得证据并回传`blocked/incomplete`。
