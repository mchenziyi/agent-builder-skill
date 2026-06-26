# Step 4.5 — 构建 Skill 封装层

> 把工具使用知识打包成可复用模块。

## Skill vs 工具

| | 工具 | Skill |
|---|---|---|
| 粒度 | 单个操作 | 多步骤流程 |
| 智能 | 无 | 含使用知识（何时调、怎么调） |
| 复用 | 函数复用 | 知识和流程复用 |
| 状态 | 无状态 | 可有状态 |

## Skill 示例

```yaml
Skill: "用户信息查询"
触发条件: 用户询问自己的信息
步骤:
  1. get_user_info 获取基本信息
  2. 如果问到订单 → get_user_orders
  3. 如果问到具体订单 → get_order_detail
工具依赖: [get_user_info, get_user_orders, get_order_detail]
安全: 所有调用前需验证身份
```
