# Phase 1: Loop Engineering

> 设计 Agent 的主循环范式。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 1.1 | [step-1-1-three-paradigms.md](step-1-1-three-paradigms.md) | 理解三种核心范式 |
| 1.2 | [step-1-2-choose-paradigm.md](step-1-2-choose-paradigm.md) | 根据场景选择主范式 |
| 1.3 | [step-1-3-loop-skeleton.md](step-1-3-loop-skeleton.md) | 实现循环骨架 |
| 1.4 | [step-1-4-termination.md](step-1-4-termination.md) | 设计终止条件 |
| 1.5 | [step-1-5-fallback.md](step-1-5-fallback.md) | 实现 Fallback 策略 |

## 协议规范

### 消息结构
每条对话消息必须符合以下契约：

| 字段 | role 取值 | 约束 |
|---|---|---|
| system | "system" | 仅首条，全局生效 |
| user | "user" | 用户输入 |
| assistant | "assistant" | 模型回复或 tool_calls 请求 |
| tool | "tool" | 必须对应一条 tool_call，必须带 tool_call_id |

- `tool_calls` 必须包含 `id`、`function.name`、`function.arguments`
- `tool` 消息必须包含 `tool_call_id`（与对应请求一致）、`role: "tool"`、`content`
- 违反以上结构 → Provider 层返回错误，不吞没不重试

### 循环终止
- 默认最大轮次 10，超过则强制终止并返回 fallback
- 连续 3 次相同 tool_call 模式 → 判定为循环，强制终止
- 单次推理超时 30s，超时返回 Timeout 错误

### Fallback 行为
- API 错误：自动重试 3 次（退避 1s/2s/4s）
- 工具失败：连续 3 次后降级为纯文本模式
- 用户中断：立即停止当前推理，返回断点状态 |

## 依赖

- 前置：无
- 被依赖：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — 完整闭环](verification.md#v1)
- [V2 — 终止条件](verification.md#v2)
- [V3 — 降级策略](verification.md#v3)

## 输出产物

- Loop Engine 模块
- 终止条件模块
- Fallback 降级模块
