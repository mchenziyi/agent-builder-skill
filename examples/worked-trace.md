# Worked Trace：AI 执行本 Skill 的参考路径

> 本文档展示 AI 如何读取本 Skill、按 Phase 执行、产出模块、编写验证。
> 不是代码示例，是**执行姿势参考**。

---

## Trace：MVP 路径（Phase 1 → 2 → 4）

### 1. AI 读 SKILL.md

```
AI 读取 SKILL.md，得到关键信息：
- 有 8 个 Phase，Phase 依赖关系见 `build-agent-system.md` 或 `SKILL.md` 中的依赖表（完整推荐路径为 1→2→3→4→5→6→7→8，但实际依赖允许并行和跳过）
- 可以先走 MVP 路径：Phase 1 → 2 → 4
- 每 Phase 完成后要跑 verification.md
```

### 2. AI 进入 Phase 1（Loop Engineering）

```
读 phase-01-loop-engineering/index.md
→ 发现有 5 个步骤
→ 开始执行 step-1-1（理解三种范式）
```

### 3. AI 执行 step-1-1

```
step-1-1 介绍 ReAct / Plan-and-Execute / Reflection 三种范式
AI 决策：
  "本项目需要实时问答 + 工具调用，选 ReAct 作为主循环范式"
AI 产出：在项目代码中实现 ReAct 循环骨架
```

### 4. AI 执行 step-1-3（实现循环骨架）

```
step-1-3 给出了循环骨架的伪代码
AI 将其翻译为目标语言（Go / Python / TypeScript 等）
AI 产出：
  - Loop Engine 模块（如 loop_engine.py）
  - 终止条件模块（termination.py）
  - Fallback 模块（fallback.py）
```

### 5. AI 跑 Phase 1 的 verification.md

```
AI 读取 verification.md 中的 V1 协议：
  "用 mock provider 替代真实 LLM"
  "mock 返回 tool_calls → 执行 → 回填 → 断言最终回复"
AI 用项目的测试框架写测试用例：
  # 例如用 Python pytest
  def test_loop_complete():
      agent = Agent(mock_provider)
      result = agent.chat("请调用工具")
      assert "工具已调用" in result
AI 运行：pytest tests/test_loop.py → 通过 ✅
→ Phase 1 完成
```

### 6. AI 进入 Phase 2（Core Architecture）

```
类似流程：
- 读 index.md → 6 个步骤
- 执行 step-2-4（Tool Registry）
- 产出 Tool Registry 模块
- 跑 verification.md → 测试通过 ✅
```

### 7. AI 进入 Phase 4（Tool System）

```
- 读 index.md → 6 个步骤
- 执行 step-4-1（JSON Schema），设计工具描述
- 执行 step-4-2（两轮对话闭环），接入 FC
- 跑 verification.md → 测试通过 ✅
```

### 8. MVP 完成

```
现在 AI 已经产出了一个"能对话、能调工具"的最小 Agent。
后续按需进入 Phase 3（Memory）、Phase 5（RAG）等。
```

---

## 关键原则

- **不要一次加载整个 Phase**：每次只读当前 step-*.md，执行完再读下一步
- **不要跳过验证**：verification.md 是判断"这一步做对了没有"的唯一标准
- **按目标语言翻译**：step 文件中的伪代码和命名只是参考，要用你的目标语言实现
- **MVP 路径例外**：Phase 1→2→4 产出最小可用 Agent，之后再补
