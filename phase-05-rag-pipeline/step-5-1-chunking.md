# Step 5.1 — 设计 Chunking 策略

**推荐：递归语义切分 + 小重叠**

```python
for doc in documents:
    paragraphs = split_by_paragraph(doc)
    for p in paragraphs:
        if token_count(p) > MAX_CHUNK_SIZE:
            sentences = split_by_sentence(p)
            chunks.extend(merge_to_target_size(sentences, MAX_CHUNK_SIZE, OVERLAP))
        else:
            chunks.append(p)
```

**参数：** MAX_CHUNK_SIZE=512-1024 tokens，OVERLAP=50-100 tokens
