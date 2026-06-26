# Step 2.1 — 明确四大核心组件

| 组件 | 职责 | 示例 |
|---|---|---|
| **Profile** | 身份、规则 | System Prompt |
| **Memory** | 记忆管理 | MemoryStore 接口 |
| **Planning** | 任务拆解、策略 | Planner 模块 |
| **Action** | 执行工具、API | ToolExecutor |

组件关系：
```
用户 → Profile → Planning → Action
                    ↕           ↓
                 Memory      LLM 推理 → 输出
```
