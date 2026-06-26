# Phase 6 验证细节

---

## V1 — Prompt Caching 效果

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 记录单次对话的 token 消耗（baseline） | — |
| 2 | 启用 Prompt Caching 后重复相同对话 | — |
| 3 | 对比 token 消耗 | 减少 > 50% |

- [ ] Token 消耗显著下降
- [ ] 回答质量无差异

## V2 — 解码参数调优

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 用 tool_call 参数（temp=0.1）调工具 10 次 | 全部正确调用 |
| 2 | 用 creative 参数（temp=0.8）生成文案 10 次 | 每次输出有差异 |
| 3 | 对比两组结果 | 工具调用稳定性明显更高 |

- [ ] 不同任务用不同参数有可感知的效果差异
- [ ] 工具调用场景稳定可靠

## V3 — 选型评估

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 列出你的核心场景和权重 | — |
| 2 | 对候选模型逐项打分 | — |
| 3 | 用真实任务跑一轮验证 | 高分模型实际表现符合预期 |

- [ ] 选型表覆盖了核心场景
## 可执行验证

```bash
# V1：Prompt Caching 效果对比
cat <<'EOF' | python3
import openai, time
client = openai.OpenAI()

# Baseline
start = time.time()
r1 = client.chat.completions.create(model="gpt-4o", messages=[
    {"role":"system","content":"你是助手。"*100},
    {"role":"user","content":"你好"}
])
base = time.time() - start

# Cached
start = time.time()
r2 = client.chat.completions.create(model="gpt-4o", messages=[
    {"role":"system","content":"你是助手。"*100},
    {"role":"user","content":"今天天气"}
])
cached = time.time() - start

print(f"Baseline: {base:.2f}s, Cached: {cached:.2f}s")
if cached < base * 0.8:
    print("✅ Prompt Caching 生效")
else:
    print("⚠️ 效果不明显，可能 provider 不支持自动缓存")
EOF

# V2：解码参数对比
cat <<'EOF' | python3
import openai
client = openai.OpenAI()

# 工具调用（低温）
r1 = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role":"user","content":"返回数字 42"}],
    temperature=0.1)
print(f"低温输出: {r1.choices[0].message.content}")

# 创意（高温）
r2 = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role":"user","content":"写一句关于春天的诗"}],
    temperature=0.8)
print(f"高温输出: {r2.choices[0].message.content}")
EOF
```
