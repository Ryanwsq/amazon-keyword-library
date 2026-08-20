# Standalone competition output contract

## Input lock

- 锁定第二板块、分类、第一板块卖家精灵批次、站点、分析周期和稳定 `Keyword_ID`。
- 只选择 Sheet2 F1–F4；不读取 SKU 事实卡。

## Minimum fields

| Group | Fields |
|---|---|
| 身份 | `Competition_Record_ID`、`Keyword_ID`、关键词、翻译、流量层、站点、周期、规则版本 |
| 来源周期 | SIF机会筛选周期、SIF竞争格局周期、卖家精灵采集周期 |
| 正式输入 | SIF CPC、Top3点击份额、Top3转化份额、市场CVR、入场信号；卖家精灵SPR、商品数量 |
| 辅助证据 | 集中度、Top1/2/3 ASIN、头部渠道结构、集中度历史、Top3外购买空间、可进入性；卖家精灵原始CPC/集中度若存在 |
| 派生 | 精确匹配状态、比较池、有效样本数、回退层级、点击转化差、头部锁定、CPC/SPR/商品数量/CVR层 |
| 结论 | 综合等级、摘要、入场信号一致性、完整性、置信度、人工复核状态 |

## Calculation and stop gate

Top3阈值、P33/P67、市场CVR修正和极高→高→中→低有序规则以项目判定边界为准。比较池从同层回退到相邻层组合和全部F1–F4；仍少于10个有效词时不计算相对层或综合等级。

满足 Top3点击/转化完整、CPC/SPR/商品数量至少两项可用、来源周期可追溯、机会筛选完整词精确匹配且样本不少于10，才可输出正式等级。其他阻断情形写`数据不足/人工复核`。

## Final-master handoff

只回传：`Competition_Record_ID`、`综合竞争等级`、`竞争判断摘要`、`竞争数据完整性`、`人工复核状态`。详细字段不写入最终总表。

## Quality gate

人口等于 Sheet2 F1–F4唯一主键数；F5/Sheet3/Sheet4零混入；正式与辅助来源未混用；比较池可复算；阻断行无综合等级；公式、主键和字段完整性检查通过。
