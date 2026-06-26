# Step 1.2 — 根据场景选择主范式

| 场景 | 推荐 | 理由 |
|---|---|---|
| 高实时、多轮对话 | ReAct | 延迟低，快速响应 |
| 多步骤、依赖链长 | Plan-and-Execute | 避免短视决策 |
| 高准确率、可纠错 | Reflection | 出错能自我修正 |
| 混合场景 | ReAct + Plan-and-Execute | ReAct 兜底，复杂子任务用 Plan |
| 生产级系统 | ReAct + Reflection | ReAct 主循环 + 关键步骤反思 |
