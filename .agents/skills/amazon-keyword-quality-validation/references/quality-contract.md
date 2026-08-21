# Independent quality-validation contract

## Run lock

验证报告必须记录 `Run_ID`、锁定仓库 revision、站点、规则/合同版本、各逻辑角色的输入输出版本、验证时间和质量报告位置。没有可锁定 revision 或阶段清单时结论为`incomplete`。

## Required gates

### Sources and first board

- SIF、Amazon联想、卖家精灵各自有状态、原始记录清单、版本和主任务回传。
- 十表存在；回传工作簿行、`Handoff_Row_ID`来源明细和机械键总表双向可回查，数量闭合；来源副任务清单中的原始事件数与聚合行数另行闭合。
- Amazon联想为必选来源；`not_executed`或适配器错误阻断第一板块完成。
- 卖家精灵四列工作簿、SIF七列工作簿、文件哈希、原始指针和紧凑回传完整；规范 ABA/搜索量、SIF Top3、回退和周期可识别，已移除字段没有混入。

### Cleaning and word frequency

- `Sheet2 + Sheet3 + Sheet4 = 原始行数`，每个 `Keyword_ID`唯一去向。
- Sheet3每行有原因，Sheet4每行有直接替代理由。
- 词频只读Sheet2；输入行、单词总次数、双词总次数和唯一词面数量闭合，原Sheet未改变，结果Sheet位于最后。

### Classification

- Sheet2/3/4派生人口分别与上游唯一主键数一致。
- F1–F5可复算；F5每词一次、一个主标签、标签内LT子组不超过20。
- Sheet2无SKU匹配/广告资格；Sheet4无竞争趋势；Sheet3无未经确认的可执行否词。

### Competition and trend

- 竞争只含Sheet2 F1–F4；正式SIF值只有完整词Top3点击与转化；绝对层、差值结构和综合等级映射可复算；Top3任一缺失或冲突未解决的行无综合等级；不存在比较池或样本门。
- 趋势只含Sheet2 F1–F3；月份非空可解析；每个主键一个矩阵列和一条图表序列；没有替代来源或趋势标签。

### Final workbook

- Sheet1行数等于Sheet2唯一主键数，与Sheet3/Sheet4交集为零。
- 竞争只回写记录ID、等级、摘要、完整性和复核状态；详细指标留独立Sheet。
- 第一、第二、第三板块实际过程Sheet全部保留，包括`Sheet5_词频统计`。
- 公式错误、重复/超长表名、空/重复主键为零；日期、长列、换行可读。
- 每张Sheet已渲染；趋势固定图表区域的关键词数、月份数、图例和序列数闭合。

## Conclusion rules

- `pass`：全部适用硬门实际执行并通过。
- `blocked`：已执行检查发现会阻止阶段完成或交付的错误。
- `incomplete`：必需来源、文件、清单、渲染或检查未执行/未提供。

不得把范围外写成缺失，不得把未执行写成零结果，不得把历史案例、迁移结构检查或planned能力写成P1通过。

## Report fields

每项检查记录：Gate ID、对象、预期、实际、证据指针、状态、影响、上游所有者、是否阻断和建议下一步。问题台账只提出修复方向，不直接修改上游文件或业务规则。
