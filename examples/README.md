# Client configuration examples

The REP AI MCP server is remote: every client points at the same address and signs in with OAuth 2.1 (PKCE) in the REP AI Console. No API key goes in these files.

| Client | File | Where it goes |
|---|---|---|
| Claude (web, desktop, mobile) | none | Settings > Connectors > search **REP AI**, or Add custom connector with `https://mcp.hellorep.ai/mcp/v1` |
| Claude Code (one user) | none | `claude mcp add --transport http rep-ai https://mcp.hellorep.ai/mcp/v1` |
| Claude Code (shared with a project) | [`../.mcp.json`](../.mcp.json) | Project root, committed to the repo |
| Cursor | [`cursor-mcp.json`](cursor-mcp.json) | `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project) |
| VS Code | [`vscode-mcp.json`](vscode-mcp.json) | `.vscode/mcp.json` in your workspace |
| Other clients | none | Add a remote (Streamable HTTP) server with the URL `https://mcp.hellorep.ai/mcp/v1` and OAuth sign-in |

After adding the server, start the sign-in from your client. The REP AI sign-in page opens in your browser: sign in, pick the store, choose permissions and click **Connect**.

Client configuration formats are owned by each client vendor and can change. If a file here stops working, check the client's MCP documentation and [open an issue](../../../issues) so we can update it.
