---
name: amazon-keyword-category-cleaning
description: Clean an Amazon keyword pool by top-level category relevance into Sheet2 category-related terms, Sheet3 excluded terms and Sheet4 direct substitutes. Use for第二板块相关性清洗、中心购买对象判断、直接替代与配件边界或行数闭环；do not use for SKU-precision classification, word-frequency rules, source collection or final publication.
---

# Amazon Keyword Category Cleaning

## 目标

依据V2.1一级品类边界把第一板块完整词池分为品类相关、其他摘除和二类词，保留事实/锚点和原因，并完成唯一主键人口闭环。

## 输入

锁定的第一板块两Sheet工作簿、Keyword_ID、规范ABA/搜索量及来源、类目锚点、自有品牌、SKU事实、清洗规则版本和输出目录。

## 输出

固定四Sheet工作簿、最小manifest和紧凑状态摘要；不复制完整源表，不生成词性/词义标签。

## 可调用能力

- `keyword.library.clean`

## 执行步骤

1. 读取项目知识、判断边界和`references/workbook-contract.md`；锁定输入名称、哈希、人口、类目锚点和清洗版本。
2. 把SKU事实卡和类目锚点卡写入`Sheet1_事实与类目锚点`。SKU事实只辅助身份和消歧，不作为删除同品类词的理由。
3. 执行流量OR门：ABA有效且<=1,000,000，或月搜索量>100。两项失败或缺失均进入Sheet3并写准确流量门状态。
4. 对通过流量门的完整词按品牌/IP/Gift、完整商品、中心购买对象、一级品类关系、直接替代条件和歧义逐层判定。
5. 同品类其他结构、外观、配置、尺寸、功能和人群进入Sheet2；自有品牌目标完整商品、可识别错字和其他语言目标完整商品不自动摘除。
6. 第三方品牌/IP、Gift、配件/零件/耗材/服务、多商品、泛词/残缺词、仅场景/人群/活动、非直接替代和无关词进入Sheet3。无法由词本身消歧时进入Sheet3并标记人工复核。
7. 只有完整商品且与一级品类解决相同主要购买任务、无需额外主要商品并通常进入同次购买比较时进入Sheet4。Sheet4允许零行。
8. 核对每个Keyword_ID恰好一个去向，`Sheet2+Sheet3+Sheet4=输入人口`；原词、指标和来源不改写，缺值不填0。
9. 扫描公式并渲染四Sheet目视复核；问题只写本轮唯一问题文档。

## 质量标准

- 工作簿恰好四Sheet且字段顺序符合合同。
- 第二板块无语义分类标签、SKU匹配或广告资格。
- Sheet2保留同品类词；Sheet3原因可解释；Sheet4只含直接替代且允许零行。
- 三去向人口、主键、来源和哈希闭合。
- Skill保持draft/planned，旧运行不冒充当前合同P1。

## 异常处理

类目锚点、自有品牌声明、输入哈希或主键无法锁定，或三去向无法闭合时停止。单词级歧义不停止整批，按Sheet3人工复核规则处理。
