---
name: amazon-keyword-quality-validation
description: Independently validate a locked Amazon keyword-library run across source, cleaning, classification, analysis and final-workbook artifacts. Use for跨阶段主键人口、版本、公式、渲染、图表和完成门验收；do not use to fix files, rerun sources, change rules or publish externally.
---

# Amazon Keyword Independent Quality Validation

## 目标

以独立只读角色复核一个锁定 Run 的跨阶段可追溯性和质量门，只报告证据与阻断项，不代替上游修复。

## 输入

锁定的 `Run_ID`、仓库 revision、各阶段输入/输出清单、规则和合同版本、本机 `.local/runs/<Run_ID>/` 产物位置、预期行数/主键/Sheet/图表清单及允许写入的质量报告目录。

## 输出

独立质量验证报告、逐门检查结果、主键/人口/版本差异、公式与渲染结果、图表闭环、敏感信息检查、问题台账和`pass/blocked/incomplete`结论。

## 可调用能力

- `keyword.quality.validate`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/end-to-end-workflow.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/quality-contract.md`，锁定 Run、revision、合同和只读范围。
2. 对照主任务 dispatch 和各副任务 return，核对逻辑角色、输入版本、实际执行模式、来源、文件清单、异常、验证和敏感信息声明；缺少清单时判`incomplete`。
3. 复核第一板块三个来源各自完成状态、原始记录与主任务十表映射。Amazon 联想`not_executed`、适配器显式错误或任一必选来源未完成时，第一板块不能通过。
4. 复核清洗三去向唯一性和行数闭环、词频输入/单词/双词/唯一词面闭环、分类人口与F5唯一主标签/LT门。
5. 复核竞争人口、精确匹配、比较池和样本门；复核趋势仅F1–F3、月份非空、矩阵列数和图表序列；不重新计算业务判定替代上游结论。
6. 复核最终工作簿的 Sheet1人口、Sheet3/Sheet4零混入、竞争写回范围、全部过程 Sheet清单、表名映射、主键、公式、可读日期、列宽/换行和词频 Sheet存在性。
7. 对每张需要验收的 Sheet 使用既有渲染或在质量目录生成独立预览，执行全 Sheet目视复核；趋势图使用固定图表区域，核对关键词数、月份数、图例和序列数。
8. 扫描质量报告和拟交付清单是否包含凭据、账号、联系人、任务 ID、绝对路径或不应进入仓库的原始业务材料；业务原件继续只留本机。
9. 输出逐门证据、准确差异和问题台账。任一硬门失败写`blocked`，必需执行未发生写`incomplete`；只有全部适用硬门通过才写`pass`。不修改任何上游文件或规则。

## 质量标准

- 质量结论可追溯到锁定 Run、revision、合同版本和具体文件。
- 全量主键、人口、版本、公式、Sheet和图表门均有结果，不用抽样替代硬门。
- `not_executed`、缺失、失败和范围外状态严格区分。
- 报告只陈述实际检查，不虚构工具执行、渲染、P1或人工复核。
- 质量 Agent不修改上游文件、不调用业务外部系统、不升级 maturity。

## 异常处理

产物不可读、版本不一致、主键/人口/Sheet清单缺失、渲染失败或合同冲突时停止对应结论并写准确阻断项；保留其他已完成检查。所有权、敏感信息或授权不清时停止读取相关材料并回传主任务或用户决定。
