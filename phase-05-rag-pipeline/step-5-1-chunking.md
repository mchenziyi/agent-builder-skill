# Step 5.1 — 设计 Chunking 策略

**推荐：递归语义切分 + 小重叠**

```
for each document:
    按段落切分
    for each 段落:
        if token_count > MAX_CHUNK_SIZE:
            按句子切分 → 合并至目标大小（含重叠）
        else:
            直接作为一个 chunk
```

**参数：** MAX_CHUNK_SIZE=512-1024 tokens，OVERLAP=50-100 tokens
