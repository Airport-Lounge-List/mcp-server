---
name: "list-lounge-brand-locations"
description: "List all Priority Pass, Capital One, Centurion, Chase Sapphire, Plaza Premium, LoungeKey or DragonPass lounges, grouped by country and airport."
---

# List every location of a lounge brand or network

List all Priority Pass, Capital One, Centurion, Chase Sapphire, Plaza Premium, LoungeKey or DragonPass lounges, grouped by country and airport.

- **Skill ID:** `list-lounge-brand-locations`
- **Tags:** travel, networks, locations, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. For a network (priority-pass, loungekey, dragonpass, plaza-premium), call `get_network_lounges` with that slug.
2. For a card-branded lounge, call `find_lounges_by_access` with `capital-one-lounge` or `chase-sapphire-lounge`. For Centurion Lounges, call `search_lounges` with the query `Centurion`.
3. Group the results by country, then airport, and give the count. For large networks, show the region the user asked about and the total, not every row.
4. Add the card or membership that gets the user in, from `list_access_methods`.

## Tools

### `get_network_lounges`

Get all lounges in a specific lounge network. Returns every lounge that accepts a particular network membership (e.g. all Priority Pass lounges worldwide). Pass airport to limit the results to one airport, e.g. the Priority Pass lounges at Heathrow.

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
        "amex-centurion",
        "diners-club",
        "star-alliance",
        "skyteam",
        "oneworld",
        "aspire"
      ],
      "description": "Network slug (e.g. priority-pass, loungekey, dragonpass, plaza-premium, amex-centurion, diners-club, star-alliance, skyteam, oneworld, aspire)"
    },
    "airport": {
      "type": "string",
      "pattern": "^[A-Za-z]{3}$",
      "description": "Optional airport IATA code (e.g. JFK, LHR). Only lounges at this airport are returned."
    }
  },
  "required": [
    "network"
  ],
  "type": "object"
}
```

### `find_lounges_by_access`

Find all airport lounges accessible with a specific credit card, membership program, airline club, alliance status, or ticket class. For example, find all lounges you can access with your Amex Platinum or Priority Pass membership. Pass airport to limit the results to one airport, e.g. the lounges an Amex Platinum card opens at JFK.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "access_method": {
      "type": "string",
      "enum": [
        "amex-platinum",
        "chase-sapphire-reserve",
        "capital-one-venture-x",
        "citi-prestige",
        "amex-centurion",
        "hilton-aspire",
        "marriott-brilliant",
        "ritz-carlton",
        "bilt-mastercard",
        "amex-business-platinum",
        "delta-reserve",
        "united-club-infinite",
        "united-explorer",
        "citi-aadvantage-executive",
        "us-bank-altitude-reserve",
        "venture-x-business",
        "priority-pass",
        "loungekey",
        "dragonpass",
        "diners-club",
        "plaza-premium",
        "capital-one-lounge",
        "chase-sapphire-lounge",
        "business-class",
        "first-class",
        "day-pass",
        "military",
        "united-club",
        "delta-sky-club",
        "admirals-club",
        "qantas-club",
        "star-alliance",
        "oneworld",
        "skyteam",
        "star-alliance-gold",
        "oneworld-emerald",
        "oneworld-sapphire",
        "skyteam-elite-plus"
      ],
      "description": "Access method slug (e.g. amex-platinum, priority-pass, chase-sapphire-reserve, business-class, star-alliance-gold)"
    },
    "airport": {
      "type": "string",
      "pattern": "^[A-Za-z]{3}$",
      "description": "Optional airport IATA code (e.g. JFK, LHR). Only lounges at this airport are returned."
    }
  },
  "required": [
    "access_method"
  ],
  "type": "object"
}
```

### `search_lounges`

Search airport lounges worldwide by name, airport name, IATA code, city, or country. Returns matching lounges with amenities, access methods, and ratings. Use this to find specific lounges or discover lounges in a location.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "query": {
      "type": "string",
      "minLength": 2,
      "description": "Search query (lounge name, airport name, IATA code, city, or country)"
    }
  },
  "required": [
    "query"
  ],
  "type": "object"
}
```

## Example requests

- Where are all the Capital One lounges?
- List the Centurion Lounge locations
- Priority Pass lounges in Japan

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
