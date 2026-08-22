# Amazon autocomplete source contract

## Fixed environment

- 首选Codex内置浏览器；当前设备无法稳定识别或操作时，允许使用普通Chrome作为备用入口。
- Amazon US、未登录、Department=`All`、配送邮编=`10001`。
- 只使用主一级品类核心大词；不描述为无痕。
- 两种浏览器入口执行同一矩阵和证据门；搜索结果页、网页搜索和API不是备用入口。

## Required matrix

- `[anchor]`、`[anchor] `。
- `[anchor] a`至`[anchor] z`。
- `a [anchor]`至`z [anchor]`。
- `[anchor] for`、`[anchor] with`、`[anchor] without`。
- 真实品类功能/配置与锚点的前后组合。
- 规格敏感时执行`[anchor] 0`至`[anchor] 9`，否则记录不适用。

## Source record

每条来源事件至少保留：来源记录 ID、Run、主锚点、触发输入、触发类型、原始建议、机械键、可见顺序、SKU探针状态、站点、Department、邮编、浏览器入口、浏览器环境、登录状态、采集时间、输入状态和证据指针。发生入口回退时另记首选入口失败状态与回退理由。

普通建议行、卡片和组内选项只有在文本完整可见时才能形成来源事件。轮播控件遮挡选项时，可在不改变输入的前提下用箭头或横向滚动揭示；保存操作前后证据并维持原组内顺序。仍未完整可见的内容只记异常；状态提示数量不等于可见建议证据。

## Gate

全部适用输入有状态，环境字段完整，成功输入保存全部完整可见建议；浏览器入口及任何回退可追溯；没有按 Enter、结果页、递归扩展、网页搜索或API替代。两个浏览器入口均不能识别/操作时整体回传`not_executed/incomplete`，不是零结果。
