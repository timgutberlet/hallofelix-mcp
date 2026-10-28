# Installing the HalloFelix von InsurMagic MCP server

This is a remote MCP server; nothing has to be installed or built.

1. Add the server to the MCP settings (Streamable HTTP):

```json
{
  "mcpServers": {
    "hallofelix": {
      "type": "streamableHttp",
      "url": "https://app.hallofelix.de/api/mcp",
      "headers": { "Authorization": "Bearer <api_access_token>" }
    }
  }
}
```

2. Ask the user for their HalloFelix API token (HalloFelix app → Integrationen → API → Token erstellen) and put it in place of `<api_access_token>`. Clients with OAuth support can omit the `headers` block and sign in with the HalloFelix login instead.
3. Verify with the `list_call_assistants` tool.
