# Airport Lounge List MCP Server

A hosted [Model Context Protocol](https://modelcontextprotocol.io) server that lets an AI assistant answer the
question every frequent flyer actually has: **which lounge can I get into here, with the cards I already hold?**

Search lounges at any airport, check access by credit card, membership, airline status or ticket class, compare
networks like Priority Pass, and — once you sign in — log visits, write reviews and keep a wishlist.

```
https://mcp.airportloungelist.com/mcp
```

There is nothing to install and no API key to copy. Add the URL to your MCP client and the tools appear.
The eight lounge-data tools work immediately, with no account.

## Install

**Claude Code**

```bash
claude mcp add airport-lounge-list --transport http https://mcp.airportloungelist.com/mcp
```

**Claude Desktop, Cursor, Windsurf, VS Code**

```json
{
  "mcpServers": {
    "airport-lounge-list": { "url": "https://mcp.airportloungelist.com/mcp" }
  }
}
```

**Gemini CLI** — this repository is a Gemini CLI extension:

```bash
gemini extensions install https://github.com/Airport-Lounge-List/mcp-server
```

**Clients without native remote-server support** can bridge with
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "airport-lounge-list": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.airportloungelist.com/mcp"]
    }
  }
}
```

## Try it

> Which lounges at Heathrow Terminal 5 can I use with Priority Pass?

> Does my Amex Platinum get me into the Centurion Lounge at LAX?

> Compare Priority Pass and DragonPass coverage in Asia.

> Log that I visited the Qantas First Lounge in Sydney yesterday and rate it 5.

## Tools

21 tools — see [TOOLS.md](TOOLS.md) for the full table with titles and descriptions.

Eight are public and need no account: `search_lounges`, `get_airport_lounges`, `get_lounge_details`,
`find_lounges_by_access`, `list_access_methods`, `list_networks`, `get_network_lounges`,
`discover_more_flight_tools`.

The other thirteen read or change your own reviews, visits and wishlist, and need OAuth.

Every tool declares MCP `title` and `annotations` (`readOnlyHint`, `destructiveHint`, `idempotentHint`,
`openWorldHint`), so a client can tell a read from a write before it calls anything.

## Transport and authentication

**Streamable HTTP.** SSE is not used.

Public tools need no token and are rate limited per IP address. Signing in lifts the limit.

Tools that touch your own data use **OAuth 2.1** with PKCE and
[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) dynamic client registration, so a compliant client
registers itself and opens a browser for consent — no keys to paste. Scopes are `read` and `write`; the eight
mutating tools require `write`.

Calling a protected tool without a token returns `401` with a `WWW-Authenticate` header pointing at the
[RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) protected-resource document, which is how your client
discovers the authorization server. `initialize`, `tools/list` and the public tools never return that challenge.

## Discovery documents

- [`/.well-known/mcp.json`](https://mcp.airportloungelist.com/.well-known/mcp.json) — server card with the full tool list
- [`/.well-known/oauth-protected-resource/mcp`](https://mcp.airportloungelist.com/.well-known/oauth-protected-resource/mcp) — RFC 9728
- [`/.well-known/oauth-authorization-server`](https://mcp.airportloungelist.com/.well-known/oauth-authorization-server) — RFC 8414
- [`/auth.md`](https://airportloungelist.com/auth.md) — the OAuth flow written for an agent to follow
- [`/.well-known/agent-card.json`](https://mcp.airportloungelist.com/.well-known/agent-card.json) — A2A agent card

## Related servers

Same team, same install pattern — each is a separate remote MCP server:

| Server | Answers | Endpoint |
|---|---|---|
| [Award Travel Finder](https://awardtravelfinder.com) | Can I redeem points for this flight? | `https://mcp.awardtravelfinder.com/mcp` |
| [FlightSeatMap](https://flightseatmap.com) | Which seat should I pick? | `https://mcp.flightseatmap.com/mcp` |
| [FlightQueue](https://flightqueue.com) | How long is security right now? | `https://mcp.flightqueue.com/mcp` |

The `discover_more_flight_tools` tool returns the same list with install snippets.

## Registry

Published to the official MCP registry as `com.airportloungelist/lounges`. See [`server.json`](server.json).

## About

Server code is not open source; this repository is the public home for the server's metadata and documentation.

Operated by **Merchant Software Solutions Limited**, registered in England and Wales, company number
[14098922](https://find-and-update.company-information.service.gov.uk/company/14098922).

- Website: <https://airportloungelist.com>
- Docs: <https://airportloungelist.com/mcp>
- Support: <hello@airportloungelist.com>
- [Privacy policy](https://airportloungelist.com/privacy) · [Terms of service](https://airportloungelist.com/legal/terms)

Documentation and metadata in this repository are MIT licensed (see [LICENSE](LICENSE)). The lounge data served
by the endpoint is proprietary and covered by the terms of service.
