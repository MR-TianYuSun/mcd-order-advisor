# MCP 集成说明 (MCP Integration)

## 1. 使用的 MCP Server

| 项目 | 内容 |
|---|---|
| 服务提供方 | 麦当劳中国（金拱门） |
| Server 名称 | `mcd-mcp` |
| 接入地址 | `https://mcp.mcd.cn` |
| 传输协议 | Streamable HTTP |
| 鉴权方式 | 请求头 `Authorization: Bearer ${MCD_MCP_TOKEN}` |
| 限频 | 600 次 / 分钟（超限返回 429） |
| 官方文档 | https://github.com/M-China/mcd-mcp-server |

## 2. 实际使用的 Tool 与调用场景

本 Skill「麦麦省钱决策引擎」围绕"自然语言需求 → 可下单的最低实付方案"主线，实际调用以下工具：

| 工具 | 在流程中的角色 |
|---|---|
| `now-time-info` | 锚定当前时段（早/午/晚/夜宵/下午茶），决定菜单范围 |
| `query-nearby-stores` | 到店 / 取餐场景选门店 |
| `delivery-query-addresses` | 外送场景读取已存地址 |
| `delivery-create-address` | 外送且无地址时新建收货地址 |
| `delivery-query-stores` | 外送场景选可配送门店 |
| `query-meals` | 获取门店可售餐品（分类 / 编码 / 标签） |
| `query-meal-detail` | 查看套餐组成与**可替换项**（饮料/酱料/配料），用于忌口求解与求解器枚举 |
| `list-nutrition-foods` | 用户关心热量 / 营养或垂直人群模式时核对成分（标注"仅供参考"） |
| `query-store-coupons` | 挖掘当前门店可用优惠券 |
| `available-coupons` | 列出麦麦省可领券 |
| `auto-bind-coupons` | 经用户同意后一键领取麦麦省全部可用券 |
| `query-my-coupons` | 调用用户已有券参与凑单 |
| `calculate-price` | **求解器核心**：按"商品 + 券"计算金额 / 配送费 / 优惠 / 应付总价（每个候选篮子必调） |
| `create-order` | 用户确认后创建订单并返回支付链接 |
| `cancel-order` / `query-order` / `order-list` | 订单取消 / 查询 / 历史复盘 |

> 未在上述链路中使用的工具（如 `mall-*` 商城积分兑换、`query-lottery-*` 抽奖、`party-*` 派对预约）本版本未调用，但 Server 已具备，后续可扩展。

## 3. 调用流程（端到端）

```
用户需求(自然语言)
   → now-time-info 锚定时段
   → [外送] delivery-query-addresses → delivery-query-stores
     [到店] query-nearby-stores
   → query-meals 取菜单
   → query-meal-detail 取套餐/替换项(去忌口/枚举)
   → [要优惠] query-store-coupons + available-coupons + query-my-coupons
             → (可选)auto-bind-coupons
   → ★ 省钱组合求解器：枚举 3-6 篮子 × calculate-price → 取最低实付
   → [多人] 份量规划/并单  [忌口] 自动替换  [人群] 蛋白/低卡/亲子排序
   → [关心营养] list-nutrition-foods
   → 输出最低实付方案 → 用户确认 → create-order
```

## 4. 业务价值（差异化）

- **真正的最低实付**：把 `calculate-price` 当"价格预言机"做组合搜索，而非"推荐后算一次"——这是 LLM+工具 独有、官方 App 做不到的能力。
- **降低决策成本**：把"翻菜单 + 比价 + 找券 + 凑单"压缩成一句自然语言。
- **多约束联合求解**：口味 / 忌口 / 预算 / 时段 / 人数 / 营养一个 prompt 同时处理。
- **垂直场景覆盖**：多人团建、忌口自动满足、健身/控糖/亲子人群模式。
- **合规可控**：所有数据来自官方 MCP，不编造；优惠与价格以官方实时返回为准。

## 5. 配置示例

脱敏后的配置见 [`mcp-config.example.json`](./mcp-config.example.json)，仅使用环境变量占位符，不含任何真实凭证。
