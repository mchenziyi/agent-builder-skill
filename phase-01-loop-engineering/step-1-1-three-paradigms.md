# Step 1.1 — 理解三种核心范式

| 范式 | 核心机制 | 适合场景 | 代价 |
|---|---|---|---|
| **ReAct** | Think → Act → Observe 循环，每步基于当前观察即时决策 | 实时问答、工具密集型 | 每轮一次 LLM 调用 |
| **Plan-and-Execute** | 先规划完整步骤，再逐条执行 | 多步骤复杂任务 | 规划阶段多耗 Token |
| **Reflection** | 执行后自我反思纠错重试 | 高准确率要求 | 增加反思轮次 |

**组合使用：** ReAct 作主循环 + 复杂子任务用 Plan-and-Execute + 关键步骤加 Reflection。
