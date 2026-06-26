# Step 6.2 — 配置解码参数

```python
def get_params(task_type):
    if task_type == "tool_call":
        return {"temperature": 0.1, "top_p": 0.2}
    elif task_type == "creative":
        return {"temperature": 0.8, "top_p": 0.95}
    elif task_type == "code":
        return {"temperature": 0.2, "top_p": 0.4}
```

**原则：** 工具调用用低温保确定性，创意用高温保多样性。
