---
name: amazon-keyword-word-frequency
description: Count and rank single-word and adjacent ordered two-word frequency from the cleaned Amazon keyword Sheet2, then append the result as the last workbook sheet. Use for第二板块词频统计、Listing撰写前词面排序、双词组合和表达顺序参考；do not use for source collection, relevance cleaning, high/medium/low tiers, ABA/search-volume weighting, SKU classification, ad decisions or direct Listing writing.
---

# Amazon Keyword Word Frequency

## 目标

只用清洗通过的 Sheet2 完整关键词，生成可追溯、可复核的单词词频和相邻有序双词词频排序表，作为 Listing 撰写的前置输入。

## 输入

- 已完成第二板块清洗并通过行数闭环的本地工作簿。
- 唯一可识别的 Sheet2 完整关键词列。
- 用户授权的输出位置；默认创建副本，不覆盖原工作簿。

## 输出

- 保留全部原 Sheet 的工作簿副本。
- 位于最后的 `Sheet5_词频统计`，包含单词词频表、双词词频表和统计口径摘要。
- 输入关键词数、单词总次数、双词总次数、不同单词数、不同双词数和校验结果。

## 可调用能力

- `keyword.library.word-frequency`

## 执行步骤

1. 完整读取 `knowledge/index.md`、`../../../docs/keyword-judgment-boundaries.md` 和 `references/workbook-contract.md`；使用当前环境的 Spreadsheets Skill 读取、编辑、检查、渲染和导出 XLSX。
2. 检查工作簿可读、Sheet2 和关键词列唯一、关键词非空、前一阶段闭环已完成，并确认没有需要覆盖的同名词频 Sheet。发现未解释的重复完整关键词时停止，不静默去重。
3. 只读取 Sheet2 关键词列。对统计副本执行 NFKC 兼容归一化和英文小写化，以空白、标点和连字符拆分词面；保留字母、数字、介词和可识别非英语词面，不覆盖原关键词。
4. 对每个拆分词面逐次累加单词频次；不删除停用词，不做单复数、近义词、拼写、词干、词序或跨语言归并。
5. 在每个完整关键词内部，按原顺序生成所有相邻双词并逐次累加；不统计非相邻共现，不交换词序。
6. 分别按重复次数降序排列单词和双词；次数相同时按标准化词面字符顺序排列。保留频次为 1 的全部词面，不设置高频、中频、低频阈值。
7. 创建工作簿副本，在最后新增 `Sheet5_词频统计`。两张表至少包含排名、词面和重复出现次数；保持原工作簿的视觉语言，并在 Sheet 中写明统计来源和不归并规则。
8. 核对单词总次数、双词总次数和唯一词面数；双词总次数必须等于每条关键词`词数 - 1`的合计。检查全部词面已输出、排序稳定、原 Sheet 未改变、无公式错误，并渲染全部工作表目视复核。
9. 交付本地工作簿和简明逻辑说明。未经明确授权，不把原始或结果 XLSX、检查日志或本机路径写入项目仓库，不 commit、不 push。

## 质量标准

- 统计输入仅来自 Sheet2，原关键词和原工作表不被覆盖。
- 单词和双词计数可从 Sheet2 机械复算，数量闭合。
- 双词严格相邻、有序；所有词面完整保留并按次数降序。
- 输出只描述本轮重复次数，不混入搜索量、ABA、广告、竞争、趋势或算法权重。
- 输出用于 Listing 撰写准备，但不替代 SKU 事实核对或 Listing 专责 Skill。
- 新 Skill 保持 `draft`，没有两个正常案例和一个边界/异常案例前不声称 `verified`。

## 异常处理

工作簿不可读、Sheet2 或关键词列无法唯一识别、存在未解释重复完整关键词、已有同名输出 Sheet、分词后出现空关键词、数量不闭合或渲染/导出失败时停止，保留原文件并报告具体问题。不要猜列、静默删词、覆盖原文件或用其他数据源补结果。
