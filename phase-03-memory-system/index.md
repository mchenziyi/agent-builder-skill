# Phase 3: Memory System

> 设计三层记忆架构。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 3.1 | [step-3-1-three-tiers.md](step-3-1-three-tiers.md) | 设计三层记忆架构 |
| 3.2 | [step-3-2-short-term.md](step-3-2-short-term.md) | 实现短期记忆 |
| 3.3 | [step-3-3-long-term.md](step-3-3-long-term.md) | 实现长期记忆 |
| 3.4 | [step-3-4-compression.md](step-3-4-compression.md) | 实现记忆压缩 |
| 3.5 | [step-3-5-granularity.md](step-3-5-granularity.md) | 设计读写粒度 |

## 协议规范

### "不要过度建设"判断准则

在实现长期记忆（3.3）和压缩引擎（3.4）之前，先判断是否真的需要：

| 场景 | 建议 | 理由 |
|---|---|---|
| 单次对话、无状态 | 仅短期记忆 | 长期记忆增加复杂度无收益 |
| 多次对话但每次独立 | 仅短期记忆 | 不需要跨会话记忆 |
| 需要记住用户偏好 | 短期 + 长期 | 长期只存偏好，不存对话历史 |
| 知识密集型问答 | 短期 + 长期 + RAG | RAG 负责知识，长期负责用户画像 |

**核心原则：** 长期记忆是 RAG 的补充，不是替代。能用 RAG 解决的问题不要用长期记忆。

### 短期记忆约束
- 默认 Token 预算 4000 tokens（含 system prompt）
- 消息优先级：P0（用户最新消息 / 工具结果）> P1（关键决策）> P2（历史记录）> P3（闲聊）
- 超出预算时，从最低优先级开始丢弃

### 长期记忆约束
- 单条记忆内容 ≤ 512 tokens
- 检索 top-K 默认 5，最多 10
- 重要性评分 < 0.3 的记忆在 7 天未访问后自动淘汰

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — 短期记忆滑动窗口](verification.md#v1)
- [V2 — 长期记忆检索](verification.md#v2)
- [V3 — 压缩效果](verification.md#v3)

## 输出产物

- 记忆系统接口模块
- 短期记忆模块
- 长期记忆模块
- 压缩引擎模块
