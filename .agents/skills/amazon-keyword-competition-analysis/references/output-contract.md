# Standalone competition output contract

## Input lock

- 锁定第二板块、分类、第一板块SIF最小字段表、站点、分析周期和稳定 `Keyword_ID`。
- 只选择 Sheet2 F1–F4；不读取 SKU 事实卡。

## Minimum fields

| Group | Fields |
|---|---|
| 身份 | `Competition_Record_ID`、`Keyword_ID`、关键词、翻译、流量层、站点、周期、规则版本 |
| 来源周期 | 第一板块SIF来源记录、必要补查批次和最新数据日期 |
| 正式输入 | SIF Top3点击份额、SIF Top3转化份额、完整词精确状态 |
| 派生 | 点击绝对层、转化绝对层、点击转化差、结构解释、头部锁定 |
| 结论 | 综合等级、摘要、完整性、置信度、人工复核状态 |

## Calculation and stop gate

Top3固定绝对阈值和两项合成头部锁定规则以项目判定边界为准。当前综合等级直接等于头部锁定度，不计算P33/P67、比较池、样本回退或其他字段修正。

满足Top3点击/转化完整、来源周期可追溯且完整词精确身份明确，即可输出正式等级。任一Top3缺失、冲突未解决或周期不明时写`数据不足/人工复核`。

## Final-master handoff

只回传：`Competition_Record_ID`、`综合竞争等级`、`竞争判断摘要`、`竞争数据完整性`、`人工复核状态`。详细字段不写入最终总表。

## Quality gate

人口等于 Sheet2 F1–F4唯一主键数；F5/Sheet3/Sheet4零混入；只存在Top3两项正式输入；绝对层和综合等级可复算；阻断行无综合等级；公式、主键和字段完整性检查通过。
