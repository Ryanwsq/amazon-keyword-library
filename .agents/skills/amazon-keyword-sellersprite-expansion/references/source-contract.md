# SellerSprite expansion source contract

## Input lock

- `Run_ID`、站点、查询月份、1–3个主任务确认的代表种子、每页20条和本机忽略目录必须锁定。
- 代表种子从第1页开始；本 Skill 不增加、替换或语义合并种子。
- `returnFields`固定为`keyword,keywordCn,searchRank,searches`，不得请求其他业务字段。

## Raw page record

本机原始事件每行至少保留：来源记录ID、批次、代表种子、Pass、页码、页内序号、站点、查询月份、采集时间、原始关键词、机械键、`keywordCn`、`searchRank`和`searches`。接口实际响应和分页包络原样保存，不重算或补造字段。

## Pagination gate

每个种子至少完成两个版本化Pass；第二Pass增加或交换机械键时最多追加第三Pass。每个Pass从第1页连续到短页或空页。总数/页数漂移只作证据，只要页码前进、响应可用且无整页重复/循环就继续；使用实际返回行数闭环。页边界重复原始行全部保留，机械键重复另行列示。

## Four-column workbook

副任务按锁定顺序机械融合所有已取得事件，工作簿业务Sheet只能包含：`英文关键词`、`中文翻译`、`ABA月排名`、`月搜索量`。同一机械键多值冲突时展示首次非空原值，不平均、不覆盖原始证据；冲突组和全部来源指针写入清单。工作簿、清单都记录Run相对路径、哈希、事件数、唯一词数、Pass状态和损失风险。

## Handoff

回传主任务：四字段工作簿、Run相对路径、文件哈希、原始事件数、唯一词数、声明总数/页数轨迹、Pass收敛、重复/冲突/缺失、损失风险、分页状态和异常摘要。逐页原始响应和原始行不进入主任务消息，只保留在 `.local/runs/<Run_ID>/keyword-sellersprite-collector/`。

## Stop

报错、限流、结构异常、缺页、整页重复、循环、无继续进展或四字段结构性缺失时停止受影响Pass，但继续其他锁定种子并装配已取得结果。单行字段空白保留空值，不构成整批结构错误，但必须进入缺失清单；只有所有种子满足Pass门时才能写`complete_with_residual_risk`。
