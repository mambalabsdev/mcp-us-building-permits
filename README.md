# US Building Permits MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-us-building-permits/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-us-building-permits) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-us-building-permits%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-us-building-permits&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-us-building-permits)](https://www.npmjs.com/package/@mambalabsdev/mcp-us-building-permits) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-us-building-permits)](https://www.npmjs.com/package/@mambalabsdev/mcp-us-building-permits) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-us-building-permits)](https://github.com/mambalabsdev/mcp-us-building-permits/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-us-building-permits)

An MCP server for the Mamba Labs US Building Permits actor on Apify. City building permits as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Reads issued building permits from the open data portals of New York, Los Angeles, Chicago, San Francisco, Austin, and Seattle, filtered by date, value, and keywords, and returns one row per permit with the contractor where the city publishes one.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-us-building-permits": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-us-building-permits"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_building_permits` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find building permits over $500,000 issued in Austin and Seattle in the last 30 days, with the contractor."
- "List roofing permits issued in Chicago since 2026-11-01 that name a contractor."

## Tool

`search_building_permits`. One tool. Example arguments:

```json
{
  "cities": [
    "austin"
  ],
  "min_value_usd": 100000,
  "max_permits_per_city": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `cities` | array | no | Cities to read: new-york, los-angeles, chicago, san-francisco, austin, seattle. Short names such as nyc, la, and sf also work. From Clay, put the column inside a one item list. |
| `issued_after` | string | no | Earliest issue date, as YYYY-MM-DD. Default: 30 days before the run. |
| `issued_before` | string | no | Latest issue date, as YYYY-MM-DD. Default: no end date. |
| `min_value_usd` | number | no | Only permits whose declared job value is at least this many US dollars, for example 50000. Permits with no declared value are left out when this is set. |
| `keywords` | array | no | Only permits whose description or type contains any of these words, for example roof, solar, new construction, pool. Case does not matter. |
| `with_contractor_only` | boolean | no | yes: only permits that list a contractor business. New York, Chicago, Austin, and Seattle publish contractors; Los Angeles and San Francisco do not, so they return a free note. Default no. |
| `max_permits_per_city` | number | no | Most permit rows per city, newest first. Default 100, up to 10000. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `permit-row`, one building permit | $0.0045 | $0.004275 | $0.00405 | $0.003825 |
| `apify-actor-start`, per run start | $0.007 | $0.007 | $0.007 | $0.007 |

Repeats, city summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/us-building-permits](https://apify.com/mambalabs/us-building-permits)

## License

MIT. See [LICENSE](LICENSE).
