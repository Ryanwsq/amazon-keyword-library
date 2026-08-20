---
name: amazon-keyword-sellersprite-expansion
description: Expand approved representative Amazon category seeds through SellerSprite from page 1 to an explicit end while preserving every raw row and field. Use for第一板块卖家精灵分页扩词、漂移重复和字段缺失检查；do not use for seed selection, SIF, autocomplete, source merging or trend queries.
---

# Amazon Keyword SellerSprite Expansion

## 目标

对主任务确认的代表种子逐页执行卖家精灵关键词挖掘，完整保留原始页、原始行、字段和分页证据。

## 输入

锁定的 `Run_ID`、站点、1–3个已去重代表种子、每页20条参数、本机忽略批次目录和当前获准的卖家精灵 MCP。

## 输出

逐种子逐页原始响应、全部行和字段、实际返回行数、机械键重复组、总数漂移、字段缺失、分页状态、异常日志及主任务回传清单。

## 可调用能力

- `keyword.source.keyword-mining.query`
- `keyword.source.sellersprite.paginate-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/source-contract.md`，核对种子、站点、分页参数和停止门。
2. 每个代表种子从第1页开始，以每页20条连续调用 `keyword.source.keyword-mining.query`；种子由主任务给定，本 Skill 不新增或替换。
3. 每页返回后立即保存完整原始响应、页码、种子、站点、时间、声明总数/页数和全部返回字段。
4. 使用 `keyword.source.sellersprite.paginate-and-verify` 检查页码连续、声明页数、明确最后页、缺页、整页重复和循环。声明总数漂移仅在页数稳定、连续、无缺页/整页重复/循环且有明确最后页时允许继续，以实际行数闭环。
5. 原始页边界重复和同一机械键冲突全部保留；只输出重复组和建议展示首条，不在本 Skill 建立三来源总表。
6. 显式保留 SPR、CPC/PPC竞价、商品数量、点击集中度及其他全部原字段。单行缺失留空并形成缺失清单，不填0、不估算、不按精确词重拉。
7. 所有种子到达明确结束后，核对逐页行数、实际总行数、重复和缺失状态，生成主任务回传清单。

## 质量标准

- 每个种子从第1页到明确结束，页码连续且原始页可回查。
- 实际返回行数是闭环真值；声明总数漂移被记录。
- 分页边界重复没有从原始证据中删除。
- 全部字段保留，SPR和商品数量的缺失状态可识别。
- 不执行种子选择、三来源合并、语义过滤、竞争补拉或趋势查询。

## 异常处理

明确报错、限流、结构异常、页数变化、缺页、整页重复、循环或无最后页时立即停止剩余页和种子，保留已取得数据作为排障证据并回传`MCP返回数据有误/blocked`。成功调用返回零结果时复核方法、站点和必填参数后形成新版本重试；未取得正常结果前不得标记完成。
