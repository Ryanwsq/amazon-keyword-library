---
name: amazon-keyword-final-workbook-assembly
description: Assemble the Amazon keyword project's final workbook with a Sheet2-only final keyword decision master as the first sheet and all first-, second- and third-board process sheets preserved. Use for最终关键词决策总表、全过程Sheet汇总、工作簿Sheet清单或跨板块主键质检；do not use for keyword collection, cleaning decisions, classification judgments, word-frequency calculation, competition data retrieval, ad eligibility, external publishing or Feishu updates.
---

# Amazon Keyword Final Workbook Assembly

## 目标

把已经完成并获准的阶段输出装配成一个最终工作簿，第一张 Sheet 便于通用词库、广告判断和后续广告模板自动取词，同时保留所有过程证据。

## 输入

- 第一板块完整采集工作簿及 Sheet 清单。
- 第二板块清洗源表、Sheet2、Sheet3、Sheet4、词频结果及其他过程 Sheet。
- 第三板块分类、二类词流量分类、否词库设计、独立竞争性分析、趋势性分析及其他过程 Sheet。
- 稳定 `Keyword_ID`、版本、站点、来源渠道和数据周期。

## 输出

- 第一张`Sheet1_最终关键词决策总表`，只收录 Sheet2 核心关键词，一词一行。
- 独立的 F1–F4 关键词竞争性分析 Sheet，保存全部竞争明细；Sheet1 只回写竞争记录引用和综合结论。
- 独立的二类词流量分类 Sheet，只收录 Sheet4 关键词。
- 独立的否词库设计 Sheet，只收录 Sheet3 关键词及否词判断。
- 独立的品类趋势性分析 Sheet，只覆盖 F1–F3，保留历年月度搜索量数据矩阵和每词一条线的折线图。
- 完整保留并加板块前缀的第一、第二、第三板块过程 Sheet。
- Sheet 清单、行数、主键、来源和版本质检结果。

## 可调用能力

- `keyword.workbook.final.assemble`
- `keyword.workbook.sheet-manifest.verify`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/workbook-contract.md`，锁定各板块版本、主键、工作表清单和缺失项。
2. 复制阶段 Sheet 到一个新工作簿，不覆盖源工作簿。给过程 Sheet 加 `B1_`、`B2_`、`B3_` 前缀；Excel 名称超过限制或重名时使用清单中的稳定短名并保存原名映射。
3. 以第二板块 Sheet2 的稳定 `Keyword_ID` 为唯一人口，建立第一张`Sheet1_最终关键词决策总表`。Sheet3 和 Sheet4 关键词不得进入。
4. 合并核心关键词的身份、翻译、来源、规范流量和分类列；多来源关键词保持一行，在`来源渠道`汇总来源集合，逐条来源事件、各来源原值、规范来源和回退标记留在第一板块过程 Sheet。F5保留`长尾主分组标签`，每个关键词只出现一次。
5. 通过 `Competition_Record_ID` 连接独立竞争 Sheet，只回写综合竞争等级、竞争判断摘要和竞争数据完整性，并把竞争复核状态与已有分类复核状态汇总到`人工复核状态`；Top3两项输入、绝对层、差值结构和派生列留在独立竞争 Sheet。F5 写`综合竞争等级=不适用（F5）`、`竞争数据完整性=范围外`，不能当作缺失。
6. 把 Sheet4 的第三板块流量分类结果保留为独立二类词 Sheet；二类词只保留 ABA 流量分层、LT 分组和替代关系，不做竞争性或趋势性分析。把 Sheet3 的否词候选设计保留为独立 Sheet；两者不与 Sheet1 混表，未确认状态不得改成可执行否词。
7. 把一个品类的 F1–F3 历年月度搜索量折线图保留为独立 Sheet，同表保留绘图数据矩阵，不在 Sheet1 增加趋势字段；F4、F5 不进入。
8. 完整保留已有过程 Sheet，包括第二板块 `Sheet5_词频统计`。若本批已完成清洗但缺少应有词频结果，停止完整装配并路由回词频 Skill；本 Skill 不自行重算或伪造 Sheet 内容。
9. 验证 Sheet1 行数等于 Sheet2 唯一 `Keyword_ID` 数、竞争明细未复制、Sheet3/Sheet4 零混入、F5主键唯一、过程 Sheet 无遗漏、跨表主键可回查、表名映射唯一和公式无错误。CSV先导入临时空工作簿再写入目标工作簿；日期显示为可读日期，ID/来源/周期/说明列设置可读列宽与换行；渲染每一张 Sheet。趋势 Sheet 使用固定图表区域预览，核对 F1–F3关键词数、非空月份数和图表序列数闭合。

## 质量标准

- Sheet1 只包含 Sheet2 核心关键词，一词一行。
- 独立竞争 Sheet 完整保留Top3两项正式输入和派生字段；Sheet1 只含 `Competition_Record_ID`、综合竞争等级、竞争判断摘要、竞争数据完整性和人工复核状态。
- Sheet3 否词库、Sheet4 二类词和 F1–F3 趋势图各自独立，不混入 Sheet1。
- 第一、第二、第三板块全部实际过程 Sheet 均被保留，新增 Sheet 也进入清单。
- 来源渠道、数据周期、版本、主键和缺失状态可追溯。
- 装配步骤不改变任何上游业务判断。
- 首轮规则回写前生成的工作簿只能作为旧运行证据；没有按新规则重跑和输出去向差异前，不得改称当前正式结果。
- Skill 保持 `draft`；未完成真实三案例前不得声称已验证。

## 异常处理

发现重复 `Keyword_ID`、Sheet2 与 Sheet3/Sheet4 交叉、应有词频或其他过程 Sheet 缺失、表名冲突、字段来源不清或行数不闭合时停止装配并输出差异清单。词频缺失时回到专责 Skill 补齐，不在装配阶段建立伪造结果。任何广告资格或广告活动填词请求均后置到本工作簿完成之后另行处理。
