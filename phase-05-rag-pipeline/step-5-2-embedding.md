# Step 5.2 — 选型 Embedding 模型

| 模型 | 维度 | 语言 | 适合 |
|---|---|---|---|
| BGE-large-zh | 1024 | 中文 | 中文知识库 |
| text-embedding-3-small | 1536 | 多语言 | 性价比高 |
| jina-embeddings-v3 | 1024 | 多语言 | 开源 |
| bge-m3 | 1024 | 多语言 | 稠密+稀疏混合 |

**选型：** 纯中文 → BGE-large-zh，多语言 → text-embedding-3-small，成本敏感 → 开源自部署。
