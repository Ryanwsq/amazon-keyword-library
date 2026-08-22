---
name: amazon-keyword-library-operations
description: Coordinate the Amazon keyword-library baseline, side-task handoffs and acceptance, including the main task's deterministic merge of three completed source handoffs. Use for项目主任务编排、三来源机械合并、版本目标、跨阶段状态或验收；do not use it to perform source extraction or business judgments owned by focused Skills.
---

# Amazon Keyword Library Operations

## 目标

锁定一次用户输入和运行版本，调度长期单一职责副任务，机械合并三来源，并控制各阶段汇合门、两对象最终交付和独立验收。

## 输入

用户产品事实和竞品、Run_ID、仓库revision、站点、规则版本、获准本机目录，以及各副任务锁定工作簿和manifest。执行三来源合并时读取`references/source-merge-contract.md`。

## 输出

- 运行输入锁、并行任务路由、阶段状态和冲突清单。
- 第一板块两Sheet业务工作簿及本地manifest。
- 最终两个顶层交付对象的汇合状态。
- 独立QA结论和可回滚版本说明。

## 可调用能力

- `keyword.library.version.manage`
- `keyword.source.merge-and-assemble`

## 执行步骤

1. 读取项目知识索引、端到端流程、判断边界和本任务直接合同，锁定Run、revision、产品事实、竞品、站点、周期和全部规则版本。
2. SIF先行；主任务把SIF候选与产品事实、直接竞品身份共同核对，锁定一个较具体的主执行锚点、全部已确认核心词、已确认等价表达及1–3个卖家精灵代表种子。主执行锚点只负责执行路由，不得把较宽泛但证据闭合的核心词降为歧义词，也不得把已确认等价表达重新当普通相邻商品；SIF流量单独存在仍不足以确认核心词或等价表达。
3. 并行调度Amazon联想和卖家精灵扩词；来源副任务按各自入口优先级执行并记录入口类型、回退和完整性。等待SIF和两个分支全部回传；副任务只回传最小工作簿、manifest和紧凑状态。
4. 按机械合同合并三来源全部已取得关键词。不得读取长响应正文做语义决定，也不得因流量或字段缺失删词。
5. 生成`Sheet1_关键词池`和`Sheet2_任务信息与类目锚点`；锚点区必须分别记录主执行锚点、已确认核心词、已确认等价表达、各自证据和完整词覆盖边界。Amazon联想`not_executed`或任一必选来源未闭合时，只能标记第一板块不完整。
6. 路由第二板块清洗；清洗闭合后并行调度词频与分类；分类完成后并行调度竞争与趋势。趋势调度固定`SellerSprite -> Sorftime`优先级，并要求同一Run全部趋势人口只使用一个锁定提供商。
7. 等待所有适用分支并锁定哈希，路由最终装配生成`过程性文件/`和七Sheet最终工作簿。
8. 路由独立质量验证执行21项门。QA未通过时不得把交付标记完成。
9. 正式只读Run中不修改知识或Skills；所有问题写同一个问题文档。整轮结束并经用户确认后，才进入获准迭代。
10. 修改知识或Skill时同批同步端到端流程；发布前执行脱敏、结构、状态和Git检查。

## 质量标准

- 并行依赖和汇合门正确，任一分支未完成不会被默认完成。
- 三来源已取得词无语义删除，规范流量和Top3来源可追溯。
- SIF/卖家精灵网页与Amazon联想普通Chrome只作为各自合同内的备用入口；入口切换不改变提供商、查询或字段，无法闭环时状态准确降级。
- 趋势使用锁定来源优先级且单Run单一提供商；实际搜索量图与比例表格职责不混合。
- 第一板块只有两个业务Sheet，主键和人口闭合。
- 主执行锚点、已确认核心词和已确认等价表达没有混为一项；宽泛核心词与等价表达的确认同时有产品事实和竞品流量候选依据，完整词改变中心商品的反例边界可传递给清洗。
- 最终顶层只有两个对象，最终工作簿恰好七个可见Sheet。
- 状态准确；旧规则工作簿不冒充当前合同结果，draft/planned不冒充P1。

## 异常处理

输入、版本、哈希、主键或人口无法锁定时停止相应汇合。来源局部失败保留已取得结果并报告损失风险；Amazon联想未执行则正常案例不完成。业务判断冲突路由拥有Skill或用户，不由主任务自行改写规则。
