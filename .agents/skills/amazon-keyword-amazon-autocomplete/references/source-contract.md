# Amazon autocomplete source contract

## Fixed environment

- Codex 内置浏览器。
- Amazon US、未登录、Department=`All`、配送邮编=`10001`。
- 只使用主一级品类核心大词；不描述为无痕。

## Required matrix

- `[anchor]`、`[anchor] `。
- `[anchor] a`至`[anchor] z`。
- `a [anchor]`至`z [anchor]`。
- `[anchor] for`、`[anchor] with`、`[anchor] without`。
- 真实品类功能/配置与锚点的前后组合。
- 规格敏感时执行`[anchor] 0`至`[anchor] 9`，否则记录不适用。

## Source record

每条来源事件至少保留：来源记录 ID、Run、主锚点、触发输入、触发类型、原始建议、机械键、可见顺序、SKU探针状态、站点、Department、邮编、浏览器环境、登录状态、采集时间、输入状态和证据指针。

## Gate

全部适用输入有状态，环境字段完整，成功输入保存全部可见建议；没有按 Enter、结果页、递归扩展或替代工具。不能识别/操作时整体回传`not_executed/incomplete`，不是零结果。
