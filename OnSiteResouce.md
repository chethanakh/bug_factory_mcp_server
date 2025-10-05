## Important Resources for Use in Sessions

- [ModelContextProtocol TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [ModelContextProtocol Inspector](https://github.com/modelcontextprotocol/inspector)

## Running a Session

1. **Set up a TypeScript project**  
    (This step is already completed.)  
    Install `tsx` for development mode.

2. **Install the ModelContextProtocol TypeScript SDK**

     ```bash
     npm install @modelcontextprotocol/sdk
     ```

3. **Create a server instance**

     ```typescript
     import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';

     const server = new McpServer({
        name: 'server-name',
        version: '1.0.0'
     });
     ```

4. **Define capabilities**

     ```typescript
     const capabilities = {
        resources: {},
        tools: {},
        prompts: {},
     };

     const server = new McpServer({
        name: "test-video",
        version: "1.0.0",
        capabilities,
     });
     ```

     > Tip: Defining capabilities separately improves code readability.

**Note:**  
Sampling is not handled by the MCP server. Implement sampling logic on the client side if needed.

5. **Use StdioServerTransport**

     ```typescript
     import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

     const transport = new StdioServerTransport();
     await server.connect(transport);
     ```

     **Why use StdioServerTransport?**  
     It enables your MCP server to communicate with the model by sending and receiving JSON messages over standard input/output streams.
