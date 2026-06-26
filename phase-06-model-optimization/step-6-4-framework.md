# Step 6.4 — 对比推理框架

| 框架 | 优势 | 适合 |
|---|---|---|
| vLLM | PagedAttention 高吞吐 | 线上高并发 |
| SGLang | RadixAttention 复杂调度 | 多轮共享前缀 |
| llama.cpp | CPU/GPU 轻量 | 本地部署 |
| TGI | HuggingFace 生态 | 快速集成 |

**选型：** 高并发 → vLLM，本地 → llama.cpp，快速集成 → TGI，极致性价比 → 直接用 API。
