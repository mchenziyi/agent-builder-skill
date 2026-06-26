# 🤖 Agent Builder Skill

> 一套结构化的 Skill 系统，让 AI 能按步骤快速搭建一个功能完整的 Agent。

## 宗旨

**让任意 AI（如 Reasonix、Claude Code、Cursor 等）读取本 Skill 后，能够按 Phase 逐步构建出一个可验证、可逐步生产化的 Agent 系统。** 从主循环到进化机制，覆盖全链路。

## 文件结构

```
agent-builder-skill/
├── SKILL.md                    ← AI 入口：执行规则 + Phase 列表
├── build-agent-system.md       ← Layer 0：Phase 目录 + 依赖关系
├── README.md                   ← 项目说明
│
├── phase-01-loop-engineering/  ← 设计 Agent 主循环
├── phase-02-core-architecture/ ← 搭建 Agent 骨架
├── phase-03-memory-system/     ← 设计记忆模块
├── phase-04-tool-system/       ← 接入工具调用
├── phase-05-rag-pipeline/      ← 构建知识检索
├── phase-06-model-optimization/← 优化推理引擎
├── phase-07-multi-agent/       ← 多 Agent 协作
└── phase-08-harness-engineering/ ← 进化机制
```

每 Phase 内部分三层：

| 层 | 文件 | 用途 | AI 何时读 |
|---|---|---|---|
| Layer 1 | `index.md` | Phase 概览，含步骤一览和验证标题 | 进入 Phase 时 |
| Layer 2 | `step-*.md` | 每个步骤的具体实现细节 | 执行到该步时 |
| Layer 3 | `verification.md` | 验证操作步骤和通过条件 | 全部步骤完成后 |

## 快速开始

AI 接入本 Skill 后的标准执行流程：

```
1. 读 SKILL.md            → 了解执行规则
2. 读 build-agent-system.md → 了解全貌
3. 进入 Phase 1，读 index.md → 了解 5 个步骤
4. 逐个执行 step-*.md      → 每步只读当前文件
5. 全部完成后跑 verification.md → 逐条验证
6. 验证通过 → 进入下一个 Phase
```

### MVP 路径

最快只需 3 个 Phase 就能跑通一个"能对话、能调工具"的最小 Agent：

```
Phase 1 (Loop Engineering) → Phase 2 (Core Architecture) → Phase 4 (Tool System)
```

## 四层渐进式披露

本 Skill 采用 **OKF（Open Knowledge Format）** + **渐进式披露**原则设计：

- **Layer 0**：目录层 — `build-agent-system.md`，8 个 Phase 一目了然
- **Layer 1**：概览层 — `index.md`，每 Phase 几步、要产什么
- **Layer 2**：实现层 — `step-*.md`，具体怎么编码
- **Layer 3**：验证层 — `verification.md`，怎么测、通过条件

AI（和人）都按需深度阅读，不浪费 Token，不遗漏关键信息。

## 许可

MIT
