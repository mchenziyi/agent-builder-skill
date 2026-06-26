# Step 4.4 — 接入 MCP 协议

> 通过 MCP Client 动态发现和调用远程工具。

## 核心流程

```
1. 连接 MCP Server（stdio / HTTP / SSE）
2. 调用 tools/list 发现可用工具
3. 映射到本地 Tool Registry
4. Agent 调用时转发 tools/call
5. 结果返回回填对话
```

## Client 实现

```go
type MCPClient struct {
    session  mcp.ClientSession
    tools    []Tool
}

func (c *MCPClient) ListTools() ([]Tool, error) {
    result, err := c.session.ListTools()
    return convertMCPTools(result.Tools), nil
}

func (c *MCPClient) CallTool(name string, args map[string]any) (any, error) {
    result, err := c.session.CallTool(name, args)
    return result.Content, nil
}
```

## 注意事项

- MCP 工具运行时动态发现，不支持静态注册
- 每个 MCP Server = 一个「工具命名空间」
- 调用前需校验参数（MCP Server 可能不帮忙校验）
