# Phase 4: Tool System

> 让 Agent 具备工具调用能力。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 4.1 | [step-4-1-json-schema.md](step-4-1-json-schema.md) | 设计 JSON Schema 工具描述 |
| 4.2 | [step-4-2-two-round-loop.md](step-4-2-two-round-loop.md) | 实现两轮对话闭环 |
| 4.3 | [step-4-3-parallel-calls.md](step-4-3-parallel-calls.md) | 实现并行工具调用 |
| 4.4 | [step-4-4-mcp.md](step-4-4-mcp.md) | 接入 MCP 协议 |
| 4.5 | [step-4-5-skill.md](step-4-5-skill.md) | 构建 Skill 封装层 |
| 4.6 | [step-4-6-security.md](step-4-6-security.md) | 设计安全边界 |

## 协议规范

### tool 消息契约
每条 `role: "tool"` 的消息必须满足：

```
role: "tool"                    // 固定值，不可用 function 或 assistant
tool_call_id: string            // 必须等于对应 tool_call.id
content: string                 // 工具返回结果的 JSON 字符串
```

- `tool_call_id` 不匹配或为空 → Provider 层应拒绝并返回错误
- `content` 最大长度 8192 tokens，超出则截断并标记 `truncated: true`
- 同一 `tool_call_id` 不可重复出现

### tool_calls 响应契约
模型返回的 `tool_calls` 必须包含：

```
id: string                      // 唯一标识
function.name: string           // 工具名，必须与注册名一致
function.arguments: string      // JSON 字符串
```

- 缺少任一字段 → 当次 tool_calls 整批拒绝，不部分执行
- `arguments` 非合法 JSON → 当次 tool_call 视为非法，继续处理其他 tool_call

### 并行调用约束
- 同一轮 tool_calls 中的各调用**必须无数据依赖**
- 检测到依赖关系（如 A 的输入依赖 B 的输出）→ 应分多轮串行
- 并行工具总数建议 ≤ 10，超过则分批执行

### 超时与重试
- 单次工具调用超时 15s，超时返回 DeadlineExceeded
- 可重试的工具（幂等操作）最多重试 2 次
- 不可重试的工具（非幂等操作）失败即返回，不重试 |

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — FC 基本调用](verification.md#v1)
- [V2 — 工具结果回填](verification.md#v2)
- [V3 — 并行调用](verification.md#v3)
- [V4 — MCP 接入](verification.md#v4)
- [V5 — 安全权限](verification.md#v5)
- [V6 — 错误降级](verification.md#v6)

## 输出产物

- Tool Registry 模块
- Tool Executor 模块
- MCP Client 模块
- Skill Loader 模块
- Security 校验模块
