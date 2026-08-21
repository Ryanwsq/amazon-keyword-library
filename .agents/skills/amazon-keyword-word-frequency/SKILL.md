---
name: amazon-keyword-word-frequency
description: Count and rank non-preposition single words and adjacent ordered two-word phrases from cleaned Amazon Sheet2 keywords. Use for第二板块词频统计、Listing写作前词面排序或介词断点双词；do not use for source collection, cleaning decisions, traffic weighting, classification, competition, trend, SKU matching or Listing writing.
---

# Amazon Keyword Word Frequency

## 目标

只用第二板块Sheet2英文关键词，删除固定英语介词并以介词为双词断点，生成可机械复算的单词和相邻双词频次。

## 输入

锁定且已通过三去向闭环的第二板块工作簿、唯一Sheet2英文关键词列、输入哈希、词频规则版本、固定英语介词表版本和输出目录。

## 输出

只含一张`词频统计`的过程工作簿、manifest和紧凑状态。Sheet内两张并列三列表：`排名、单词、出现次数`与`排名、相邻双词、出现次数`。

## 可调用能力

- `keyword.library.word-frequency`

## 执行步骤

1. 读取知识、判断边界和`references/workbook-contract.md`；锁定Sheet2人口、关键词列、哈希、规则版本和介词表版本。
2. 只读取非空Sheet2英文关键词。对统计副本执行NFKC和英文小写，以空白、标点和连字符分隔；不覆盖原词。
3. 从单词序列删除固定介词；保留数字、冠词、连词、其他停用词和可识别非英语词面，除非它们属于介词表。
4. 单词频次只累计非介词token。
5. 将介词作为硬断点，在每个连续非介词词段内部生成相邻有序双词；不跨介词、不统计非相邻、不交换顺序。
6. 两表按次数降序、同次数词面字符升序；保留次数1。
7. 验证单词总数和每个连续非介词词段的`max(词数-1,0)`双词闭环，检查无介词词或含介词/跨介词双词。
8. 写入独立过程工作簿和manifest，扫描公式、渲染Sheet并目视复核。

## 质量标准

- 输入只来自Sheet2英文关键词，源工作簿不变。
- 单词表无介词，双词不含介词且不跨介词。
- 计数、排序和唯一词面可机械复算，次数1保留。
- 输出不含ABA、搜索量、流量层、权重、竞争、趋势或广告结论。
- Skill保持draft/planned，未完成真实三案例不称verified。

## 异常处理

Sheet2/关键词列/哈希/介词表版本无法唯一锁定、主键人口不闭合、分词后异常、计数不闭合或渲染失败时停止并保留源文件。
