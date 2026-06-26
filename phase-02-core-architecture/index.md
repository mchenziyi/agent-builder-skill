# Phase 2: Core Architecture

> 搭建 Agent 骨架。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 2.1 | [step-2-1-four-components.md](step-2-1-four-components.md) | 明确四大核心组件 |
| 2.2 | [step-2-2-three-layers.md](step-2-2-three-layers.md) | 区分三层职责 |
| 2.3 | [step-2-3-system-prompt.md](step-2-3-system-prompt.md) | 设计 System Prompt |
| 2.4 | [step-2-4-tool-registry.md](step-2-4-tool-registry.md) | 实现 Tool Registry |
| 2.5 | [step-2-5-session.md](step-2-5-session.md) | 设计 Session 管理 |
| 2.6 | [step-2-6-provider.md](step-2-6-provider.md) | 实现 Provider 抽象层 |

## 协议规范

### Provider 最低接口

```
Chat(messages, tools) → Response
  messages: Message[]           // 符合消息结构契约
  tools: Tool[]                 // JSON Schema 数组，可选
  Response: {
    content: string,
    finish_reason: "stop" | "tool_calls" | "length",
    tool_calls?: ToolCall[]
  }

ChatStream(messages, tools) → Stream<Response>
  // 必须支持，用于实时输出
  // 每 chunk 的 finish_reason 仅最后一个 chunk 有意义

IsAvailable() → bool
  // 健康检查，用于 failover 决策
```

### 约束
- `Chat` 必须同时支持 streaming 和 non-streaming
- failover 超时：主 Provider 15s，备用 Provider 30s
- `finish_reason` 仅可为上述三种值，不可出现自定义值
- 所有 Provider 实现必须是线程安全的

### Tool Registry 约束

```
Register(tool: Tool)
  name: 小写 + 下划线，唯一
  description: ≤ 1024 字符

Execute(name, args) → (result, error)
  args 必须通过 JSON Schema 校验
  不支持分组执行（一次 Execute 只调一个工具）
  不存在的 name → 返回 NotFound 错误
```

### Session 约束
- `Session.ID` 必须全局唯一（UUID 或 ULID）
- `Session` 存储的 Messages 总量建议 ≤ 100 条，超出触发压缩
- 序列化格式应对称（JSON 序列化后反序列化应得到相同对象）

## 依赖

- 前置：[Phase 1](../phase-01-loop-engineering/index.md)
- 被依赖：Phase 3、Phase 4、Phase 5、Phase 7

## 验证标准

- [V1 — 四大组件](verification.md#v1)
- [V2 — Tool Registry](verification.md#v2)
- [V3 — Provider failover](verification.md#v3)

## 输出产物

- Profile / Prompt 管理模块
- Tool Registry 模块
- Session 管理模块
- Provider 抽象层模块
