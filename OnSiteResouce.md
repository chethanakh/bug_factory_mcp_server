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

    6. **Install and Use ModelContextProtocol Inspector**

        Install the Inspector as a development dependency:

        ```bash
        npm install @modelcontextprotocol/inspector --save-dev
        ```

        To run your MCP server with the Inspector, use:

        ```bash
        npx @modelcontextprotocol/inspector npm run server:dev
        ```

        > **Why set `DANGEROUSLY_OMIT_AUTH=true`?**  
        > This environment variable tells the MCP Inspector (or other MCP client tools) to skip authentication when connecting to your MCP server. Use this only in development environments.

        For convenience, add this script to your `package.json`:

        ```json
        "scripts": {
          "server:inspect": "set DANGEROUSLY_OMIT_AUTH=true && npx @modelcontextprotocol/inspector npm run server:dev"
        }
        ```

    7. **Build a Tool**

        Example: Register a `create-user` tool on your server.

        ```typescript
        server.tool(
          "create-user",
          "Create a new user in the database",
          {
             name: z.string(),
             email: z.string(),
             address: z.string(),
             phone: z.string(),
          },
          {
             title: "Create User",
             readOnlyHint: false,
             destructiveHint: false,
             idempotentHint: false,
             openWorldHint: true,
          },
          async params => {
             try {
                const id = await createUser(params);
                return {
                  content: [{ type: "text", text: `User ${id} created successfully` }],
                };
             } catch {
                return {
                  content: [{ type: "text", text: "Failed to save user" }],
                };
             }
          }
        );
        ```

        | Property            | Description                              | Example         |
        |---------------------|------------------------------------------|-----------------|
        | **title**           | Name of the tool                         | "Create User"   |
        | **readOnlyHint**    | `false` if the tool modifies data        | `false`         |
        | **destructiveHint** | `false` if not deleting/destroying data  | `false`         |
        | **idempotentHint**  | `false` if repeated calls may duplicate  | `false`         |
        | **openWorldHint**   | `true` if depends on external systems    | `true`          |

8. How to add mcp to vs code

Cmd + shift + P 


9. add new resource

```
  new ResourceTemplate('greeting://{name}', {
    list: async () => ({
      resources: [
        {
          name: 'world',
          uri: 'greeting://world',
          title: 'World Greeting',
          description: 'A greeting for the world'
        }
      ]
    })
  }),
```
