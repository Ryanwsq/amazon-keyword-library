# Amazon Keyword Library

本仓库是 Amazon 关键词词库搭建项目的独立、可审查工作区，负责从真实产品输入开始完成关键词采集、清洗、词频、分类、竞争性分析、趋势性分析和最终工作簿装配。

## 当前阶段

- 仓库状态：单一职责 Skill 拆分与独立质量验证包已完成本机 P0；十个长期副任务已在本设备创建并登记到 Git 忽略映射；首个固定 Git 基线已建立。
- 当前任务：`Amazon关键词词库｜主任务｜main`。
- 远端状态：私有仓库 `Ryanwsq/amazon-keyword-library`，唯一长期基线分支为 `main`。
- 首次正式验证：尚未开始；下一步以用户提供的新真实案例执行固定版本只读验证。
- Skill 状态：当前12个包全部为 `draft`，能力均为`planned`；结构拆分和P0不构成 P1。

## 主要入口

- 项目状态：`PROJECT.md`
- 项目规则：`AGENTS.md`
- 端到端流程：`docs/end-to-end-workflow.md`
- 任务架构：`docs/thread-architecture.md`
- 任务角色：`docs/thread-roles.md`
- 判定边界：`docs/keyword-judgment-boundaries.md`
- 知识索引：`knowledge/INDEX.md`
- 项目 Skills：`.agents/skills/`

## 数据边界

真实产品资料、ASIN、原始接口响应、业务工作簿、浏览器状态、任务 ID、绝对路径、Token 和账号信息只保存在本机忽略目录，不进入 Git。仓库只保存脱敏、稳定、可复用并经确认的规则、Skills、知识、合同和真实验证证据摘要。
