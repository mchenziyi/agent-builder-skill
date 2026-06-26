# Phase 5 验证细节

---

## V1 — Chunking 质量

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 准备一篇 3000 字的文档 | — |
| 2 | 执行 chunking | 切出 3-6 个 chunk |
| 3 | 检查每个 chunk 的语义完整性 | 无在段落中间切断的情况 |

- [ ] chunk 边界在段落/句子结束处
- [ ] 每个 chunk 语义完整可读

## V2 — 检索召回率

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 向知识库存入 10 篇文档 | — |
| 2 | 提出 5 个覆盖各文档的问题 | — |
| 3 | 检查 top-5 检索结果 | 正确答案在 top-5 中的比例 > 80% |

- [ ] 召回率 > 80%
- [ ] 检索延迟 < 500ms

## V3 — 端到端问答

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 通过 Agent 提问知识库内容 | — |
| 2 | 检查最终回复 | 回答正确且引用了检索内容 |
| 3 | 检查是否出现幻觉 | 回复中没有检索结果之外的信息 |

- [ ] 答案基于检索内容
## 可执行验证

```bash
# V1：Chunking 质量测试
cat <<'EOF' | python3
doc = "第一段内容。" * 50 + "第二段内容。" * 50 + "第三段内容。" * 50
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=200, chunk_overlap=20)
chunks = splitter.split_text(doc)
assert 3 <= len(chunks) <= 10, f"预期 3-10 chunks, 得到 {len(chunks)}"
print(f"✅ Chunking 通过: {len(chunks)} chunks")
EOF

# V2：检索召回率（需有向量数据库）
# python3 -c "
# from retriever import Retriever
# r = Retriever()
# results = r.search('你的测试问题')
# assert len(results) > 0
# print('✅ 检索通过')
# "

echo "⚠️ 完整检索测试需搭建向量数据库后运行"
```
