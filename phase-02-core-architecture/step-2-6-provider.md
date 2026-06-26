# Step 2.6 — 实现 Provider 抽象层

Provider 最低接口：

```
Provider {
    chat(messages, tools) → Response
    chat_stream(messages, tools) → Stream<Response>
    is_available() → bool
}

// failover：主模型失败 → 切备用
function chat_with_failover(request):
    for each provider in providers:
        if provider.is_available():
            return provider.chat(request)
    return error("所有 Provider 不可用")
```

**典型配置：** 1. 主力模型 → 2. 快速模型（降级）→ 3. 备用 Provider（保底）

## 前置条件

- Session 管理已实现（step-2-5）
- 已确定至少一个目标 Provider 的 API 接入方式

## AI 必须产出

- Provider 抽象层模块（含接口定义 + 至少一个实现 + failover）
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：`Chat(messages, tools)`、`ChatStream(messages, tools)`、`IsAvailable()`
- **输出**：Response（含 content、finish_reason、tool_calls）
- **错误**：Provider 不可用时返回 Unavailable 错误；Chat 超时（15s）返回 Timeout
- **不变量**：所有 Provider 实现必须是线程安全的；ChatStream 必须支持（即使 fallback 到非 streaming）

## 完成定义

- 至少对接一个真实 Provider（可通过配置开关）
- failover：主 Provider 失败后自动切换
- 对应 verification.md V3 测试通过
