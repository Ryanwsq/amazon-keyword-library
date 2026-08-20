---
name: amazon-keyword-classification
description: Classify locked Amazon Sheet2 and Sheet4 keywords by confirmed ABA traffic layers and semantic structure, and prepare the Sheet3 negative-keyword candidate structure. Use for第三板块F1-F5流量分层、LT分组、词性词义多标签、二类词流量分类或分类过程表质检；do not use for source collection, category cleaning, word-frequency calculation, SKU-fact matching, competition/trend analysis, final ad eligibility, automatic negative targeting or external publication.
---

# Amazon Keyword Classification

## 目标

在不改变第二板块三去向身份的前提下，生成可供竞争分析、趋势分析和最终工作簿装配使用的第三板块分类过程表。

## 输入

- 锁定的第二板块版本、Sheet2/3/4 原始行数和稳定 `Keyword_ID`。
- 原词、中文翻译、ABA 排名、搜索量、来源渠道、站点和数据周期。
- Sheet4 的中心购买对象、二类商品类型和直接替代理由。
- `Sheet5_词频统计`版本只作流程血缘和最终过程 Sheet 保留；本 Skill 不读取词频值或据此判断。
- 分类规则版本；SKU 事实卡不属于输入。

## 输出

- `第三板块_Sheet2关键词分类`：Sheet2 一词一行的 F1–F5、词性/词义多标签、F5长尾主分组标签和标签内 LT 分组。
- `第三板块_Sheet4二类词流量分类`：Sheet4 一词一行的 F1–F5、LT 分组和替代关系。
- `第三板块_Sheet3否词候选设计`：Sheet3 原因、候选状态、误伤检查和人工复核字段；未确认边界下不自动形成精准/词组否定结论。
- 分类范围、主键、行数、异常和复核清单。

## 可调用能力

- `keyword.library.classify`
- `keyword.classification.sheet2.apply`
- `keyword.classification.sheet4.apply`
- `keyword.classification.sheet3.prepare`
- `keyword.classification.outputs.write-and-verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/output-contract.md`，锁定版本、三张表人口、主键、周期和未确认项；不读取 SKU 事实卡。
2. 复制 Sheet2/3/4 到新的第三板块过程表，不覆盖第二板块原表，不新增或删除关键词，也不改变三去向身份。
3. 对 Sheet2 的有效 ABA 排名应用互斥层：F1=`1–10,000`、F2=`10,001–20,000`、F3=`20,001–50,000`、F4=`50,001–100,000`、F5=`>100,000`。F2 的显示名固定为“二级流量词”。
4. 对 Sheet2 完整关键词做多标签分类。只使用关键词文字明确支持的基础维度：品类/产品、细分类型/结构、功能/卖点、颜色/图案/风格、材质、尺寸/规格、承重/容量/功率/数量/参数、人群、场景/用途、兼容对象/型号、需求/痛点、其他语言、错字错词、自有品牌词、其他/未知。一个词可以有多个标签；不依据 SKU 是否具备某属性删词、判匹配或判广告资格。
5. 仅对 F5 选择一个`长尾主分组标签`，优先级固定为：承重/容量/功率/数量/参数、尺寸/规格、颜色/图案/风格、人群、功能/卖点、材质、细分类型/结构、场景/用途、其他语言、错字错词、品类/产品。保留全部多标签，但每个关键词只出现一次。
6. 在每个长尾主分组标签内按 ABA 升序排序；该标签组超过 20 个词时再拆 `LT-1`、`LT-2`……，每个子组最多 20 个。流量排序优先于标签内其他选择条件。第二板块没有 ABA 但有规范搜索量的词保留`ABA未提供`状态，不伪造排名或 LT 组。
7. 对不能稳定归入基础维度的词使用`其他/未知`。类目专属等价表达只有在本批次得到明确确认后才可追加，并记录标签版本；不得把历史案例具体词固化为跨类目词典。多标签本身不视为冲突，F5 只按固定优先级选主分组标签。
8. 对 Sheet4 使用同一 F1–F5 与 F5主标签/LT 规则，只补充流量层、主分组标签和长尾组，继续保留中心购买对象、二类商品类型和直接替代理由。不得加入竞争或趋势字段，也不得因流量变化改写二类词身份。
9. 对 Sheet3 只建立否词候选设计结构，保留原摘除原因、候选状态、拟否定对象、误伤检查、判断依据和人工复核字段。在精准/词组否定边界确认前，候选状态统一保持`需人工确认`或已有上游明确状态，不自行决定发布级否词。
10. 检查 Sheet2、Sheet3、Sheet4 派生表分别与上游人口一一对应，主键唯一且可回查；检查 F2/二类词术语、F5 每词只出现一次且标签子组不超过20、Sheet4 排除竞争/趋势和 Sheet3 未产生可直接投放结论。

## 质量标准

- 第二板块三去向身份、原词、原始指标和来源字段均未被覆盖。
- Sheet2、Sheet4 的有效 ABA 值可复算到唯一 F1–F5；F5保留全部多标签、恰有一个主分组标签、每词只出现一次且每个标签子组最多20个词。
- Sheet2 多标签只描述关键词自身语义，不出现 SKU 匹配、广告资格或行动建议。
- Sheet4 与 F2 术语严格分开，且不进入竞争/趋势。
- Sheet3 仍是候选设计表，不冒充可直接上传的否词库。
- Skill 保持 `draft`；没有两个正常案例和一个边界案例的真实 P1 前不得标记 `verified`。

## 异常处理

缺少稳定主键、版本或人口基线时停止。有规范搜索量但无 ABA 时写`ABA未提供`并保留分类人口，不伪造流量层或 LT 组；F5主标签不能确定时按固定优先级计算，仍无可用标签才写`其他/未知`。词组/精准否定方式、误伤范围或发布层级未确认时只输出候选结构，不给自动否定结论。应有词频过程 Sheet 缺失时标记流程不完整并路由到词频 Skill；不得在本 Skill 重算、伪造或用词频影响分类结果。
