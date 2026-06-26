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

```
class MCPClient:
    session: MCPClientSession
    tools: Tool[]

    function list_tools() → Tool[]:
        result = this.session.list_tools()
        return convert_mcp_tools(result.tools)

    function call_tool(name, args) → any:
        result = this.session.call_tool(name, args)
        return result.content
```

## 注意事项

- MCP 工具运行时动态发现，不支持静态注册
- 每个 MCP Server = 一个「工具命名空间」
- 调用前需校验参数（MCP Server 可能不帮忙校验）
