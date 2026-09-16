# mcp-bitquery

Bitquery MCP — trade-level blockchain data (DEX trades, transfers, holders)

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1576+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `bitquery_dex_trades` | Recent DEX trades for a token — swaps involving an ERC-20 token across DEXs on an EVM chain (Bitquery). Returns trade time, DEX/protocol, buy & sell currency + amounts, price, tx hash, and maker. Example: bitquery_dex_trades({ token: "0xdAC17F958D2ee523a2206206994597C13D831ec7", network: "eth", limit: 20, _apiKey: "your-bitquery-token" }) |
| `bitquery_token_transfers` | Recent transfers of a token — ERC-20 transfer events for a token contract on an EVM chain (Bitquery). Returns transfer time, sender, receiver, amount, and tx hash. Example: bitquery_token_transfers({ token: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", network: "eth", limit: 20, _apiKey: "your-bitquery-token" }) |
| `bitquery_token_holders` | Top holders of a token — largest current balances for an ERC-20 token contract on an EVM chain (Bitquery Holders cube). Returns holder address and balance. Optionally scope to a snapshot date. Example: bitquery_token_holders({ token: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", network: "eth", limit: 20, _apiKey: "your-bitquery-token" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "bitquery": {
      "url": "https://gateway.pipeworx.io/bitquery/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/bitquery/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1576+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "bitquery": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-bitquery"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-bitquery
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Bitquery data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
