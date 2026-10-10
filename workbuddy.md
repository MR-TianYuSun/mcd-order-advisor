# WorkBuddy 开发上下文 (workbuddy.md)

> 本文件用于核验本项目是否符合「麦当劳程序员创意开发大赛」WorkBuddy 专项联动奖励条件：项目真实使用腾讯 WorkBuddy 智能体完成开发。

## 开发环境

- 开发工具：腾讯 WorkBuddy（内置 AI 助手 + 连接器 + 技能系统）
- 开发方式：在 WorkBuddy 对话中完成需求分析、架构设计、Skill 编写与文件产出
- 参赛者身份：WorkBuddy 用户（孙天愉）

## 开发过程关键节点

### 1. 赛事解析（WorkBuddy 联网抓取官方仓库）

通过 WorkBuddy 抓取并分析了以下官方资料，确认参赛规则：

- 主仓库 `M-China/mcd-developer-innovation-challenge` README：报名时间（2026-10-09 ~ 10-25）、排名机制（GitHub 公开 Star 数）、奖励结构。
- `activityGuidelines.md`：完整活动规则、并列排名处理、奖品发放与合规要求。
- `CONTEST_DECLARATION.md`：参赛声明原文（一字未改提交）。
- `M-China/mcd-mcp-server` README：麦当劳 MCP Server 接入地址、协议、限频，以及**全部约 35 个 Tool 列表**（点餐、优惠、营养、门店、商城、抽奖、派对、订单等）。

### 2. 方向决策（WorkBuddy 结构化方案 + 用户选择）

WorkBuddy 给出「项目方向 × 技术形态」矩阵方案，用户最终选定：

- 项目方向：**麦麦点餐决策官**（自然语言需求 → 点餐/省钱方案）
- 技术形态：**WorkBuddy Skill 智能体**（交付 SKILL.md + 文档，契合官方合作、拿 3000 积分）

### 3. 资产产出（WorkBuddy 直接生成）

WorkBuddy 在对话中一次性产出参赛仓库全部文件：

- `SKILL.md`：角色定义、硬约束、mcd-mcp 工具清单与用法、决策工作流、输出格式、合规护栏。
- `README.md`：痛点引入、功能亮点、快速开始、对话示例、技术架构、目录结构、合规免责。
- `CONTEST_DECLARATION.md`：官方原文原样提交。
- `MCP_INTEGRATION.md`：实际使用的 Server、Tool、调用流程、业务价值。
- `mcp-config.example.json`：仅含 `${MCD_MCP_TOKEN}` 占位符。
- `references/decision-workflow.md`：需求抽取字段表、门店分支、优惠三路并行、计价下单与合规护栏。

### 4. 合规自检

- 已确认 `CONTEST_DECLARATION.md` 内容与官方原文完全一致，未做任何修改。
- 已确认仓库内无任何真实 Token / 密钥，配置仅使用环境变量占位符。
- 已确认 Skill 提示词包含：非官方产品声明、不贬低品牌、营养数据"仅供参考"、下单前用户确认等合规护栏。

## 结论

本项目在需求分析、规则对齐、架构设计与全部代码/文档产出环节均通过 WorkBuddy 完成，符合 WorkBuddy 专项联动奖励的"真实使用腾讯 WorkBuddy 智能体开发"条件。
