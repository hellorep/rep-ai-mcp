# Security Policy

REP AI takes the security of the REP AI MCP server (`https://mcp.hellorep.ai/mcp/v1`) and the data it serves seriously. REP AI is SOC 2 Type II compliant.

## Reporting a vulnerability

Please **do not** open a public GitHub issue for security problems.

Email **support@hellorep.ai** with the subject line `Security: REP AI MCP server` and include:

- A description of the issue and its potential impact
- Steps to reproduce, including the MCP client and the tool calls involved
- Any proof-of-concept, logs or screenshots (remove shopper personal data)

We will acknowledge your report, keep you updated while we investigate, and credit you once a fix ships if you'd like.

## Scope

In scope:

- The hosted MCP endpoint at `mcp.hellorep.ai`
- The OAuth 2.1 sign-in and consent flow in the REP AI Console
- Permission (scope) enforcement for read and change tools
- The propose / approve / apply flow for changes, including expiry and single-use enforcement
- Prompt injection that causes shopper-written content to be treated as an instruction or an approval

Out of scope:

- Vulnerabilities in third-party MCP clients (report those to the client vendor)
- Denial-of-service or volume testing
- Social engineering of REP AI staff or merchants

## Safe harbor

We will not take legal action against good-faith research that respects merchant and shopper privacy, avoids data destruction and service disruption, and gives us reasonable time to fix the issue before disclosure.
