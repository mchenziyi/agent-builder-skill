# Step 6.1 — Prompt Caching / KV Cache

KV Cache 复用重复输入的 Key-Value 矩阵，多轮对话省 50-90% Token 费用。

| 场景 | 节省 |
|---|---|
| 多轮对话（5+ 轮） | 50-90% |
| 固定 System Prompt | 80-90% |
| 长文档处理 | 30-50% |

**支持情况：** DeepSeek 自动缓存，OpenAI 需设 `ephemeral`，Claude 自动缓存。

> 以目标 Provider 当前文档为准，缓存策略可能随时变化。
