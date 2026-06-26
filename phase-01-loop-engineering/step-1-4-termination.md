# Step 1.4 — 设计终止条件

| 条件 | 实现 | 优先级 |
|---|---|---|
| 最大轮次 | `rounds > MAX_ROUNDS`（建议 5-10） | 硬限制 |
| 正常终止 | `finish_reason == "stop"` | 正常 |
| 用户中断 | 检查中断信号 | 最高 |
| 循环检测 | 连续 3 次相同 tool_call 模式 | 安全机制 |
| 超时 | `elapsed > TIMEOUT` | 硬限制 |
