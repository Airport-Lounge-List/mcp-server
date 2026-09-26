---
name: compare-lounge-networks
description: "Compare Priority Pass, LoungeKey, DragonPass, Plaza Premium and airline networks on coverage, cost and access rules."
---

# Compare lounge networks

Compare Priority Pass, LoungeKey, DragonPass, Plaza Premium and airline networks on coverage, cost and access rules.

- **Skill ID:** `compare-lounge-networks`
- **Tags:** travel, networks, comparison, priority-pass
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `list_networks` for descriptions and total lounge counts.
2. Call `get_network_lounges` for each network in the comparison and count lounges by the region or airports the user named.
3. Present a short table: network, lounges in scope, notable airports.

## Tools

### `list_networks`

List all lounge networks (Priority Pass, LoungeKey, DragonPass, Plaza Premium, Amex Centurion) with their descriptions and total lounge counts.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
  },
  "type": "object"
}
```

### `get_network_lounges`

Get all lounges in a specific lounge network. Returns every lounge that accepts a particular network membership (e.g. all Priority Pass lounges worldwide).

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "network": {
      "type": "string",
      "enum": [
        "priority-pass",
        "loungekey",
        "dragonpass",
        "plaza-premium",
        "amex-centurion"
      ],
      "description": "Network slug (priority-pass, loungekey, dragonpass, plaza-premium, amex-centurion)"
    }
  },
  "required": [
    "network"
  ],
  "type": "object"
}
```

## Example requests

- Priority Pass vs LoungeKey — which has better coverage in Asia?
- How many airports does DragonPass cover?

## Authentication

None. Call the endpoint anonymously.
