---
name: "airline-lounge-access"
description: "Explain how to get into Delta Sky Clubs, United Clubs, Admirals Clubs and alliance lounges, and find them at an airport."
---

# Get into an airline lounge

Explain how to get into Delta Sky Clubs, United Clubs, Admirals Clubs and alliance lounges, and find them at an airport.

- **Skill ID:** `airline-lounge-access`
- **Tags:** travel, airlines, status, alliances, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `list_access_methods`. The airline slugs are delta-sky-club, united-club, admirals-club, qantas-club, star-alliance-gold, oneworld-emerald, oneworld-sapphire and skyteam-elite-plus.
2. Find the cards whose `includes_access` lists the airline slug, for example amex-platinum for delta-sky-club, and name them as ways in.
3. Call `find_lounges_by_access` with the airline slug and the user's airport to list the clubs there.
4. Say that airline clubs often need a same-day boarding pass on that airline or a partner, and point the user to the airline for current visit limits.

## Tools

### `list_access_methods`

List all available lounge access methods including credit cards (Amex Platinum, Chase Sapphire Reserve, etc.), membership programs (Priority Pass, LoungeKey, etc.), airline clubs, alliance status levels, and ticket classes.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
  },
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

## Example requests

- How do I get into a Delta Sky Club at ATL?
- Does my Amex Platinum get me into the Sky Club?
- Which United Clubs are at ORD?

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
