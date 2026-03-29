# Thinking Toolkit MCP

A remote MCP (Model Context Protocol) server deployed on Cloudflare Workers that exposes 12 structured thinking techniques for diagnosing and resolving stuck-ness.

## What it does

When Claude (or any MCP client) encounters a problem, it can call this server to:

1. **Diagnose** the type of stuck-ness from a problem description
2. **Load** the full methodology for the recommended technique
3. **Browse** all available techniques
4. **Reason through** problems using structured frameworks

## Tools

| Tool | Purpose |
|------|---------|
| `list_techniques` | List all 12 techniques with descriptions and trigger feelings |
| `diagnose` | Describe a problem, get diagnosed stuck-type + full methodology |
| `get_technique` | Load a specific technique's full methodology by ID |
| `get_thinking_toolkit` | Load the master diagnostic router and decision tree |

## The 12 Techniques

| Technique | Trigger Feeling |
|-----------|----------------|
| Cause-Effect Confusion | "Am I solving the right problem?" |
| Temporal Blindness | "Does the order/timing matter?" |
| Collision-Zone Thinking | "I've tried everything obvious" |
| Inversion Exercise | "We have to do it this way" |
| Meta-Pattern Recognition | "I keep seeing this pattern" |
| Scale Game | "Will this scale? Edge cases?" |
| Simplification Cascades | "It keeps getting more complex" |
| Perspective Mapping | "Why don't they understand?" |
| Contradiction Holding | "Both of these seem true" |
| Feedback Loop Mapping | "Why does this keep happening?" |
| Priority Paralysis | "Everything feels equally important" |
| Decision Paralysis | "I can't decide / cross the threshold" |

## Setup

```bash
npm install
npm run dev     # local dev at http://localhost:8787
npm run deploy  # deploy to Cloudflare Workers
```

## Connecting

### Smithery (Remote HTTP)

Add the deployed Worker URL as a remote MCP server:
```
https://thinking-toolkit-mcp.<your-account>.workers.dev/sse
```

### Claude Desktop

```json
{
  "mcpServers": {
    "thinking-toolkit": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://thinking-toolkit-mcp.<your-account>.workers.dev/sse"
      ]
    }
  }
}
```

### MCP Inspector

```bash
npx @modelcontextprotocol/inspector@latest
# Enter: http://localhost:8787/sse (local) or deployed URL
```

## Architecture

- **Runtime:** Cloudflare Workers + Durable Objects
- **Protocol:** MCP over SSE (Server-Sent Events)
- **Auth:** None (authless)
- **Storage:** All 12 skill methodologies are bundled into the Worker (~260KB)
- **Framework:** Cloudflare Agents SDK (`McpAgent`)
