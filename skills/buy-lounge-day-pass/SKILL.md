---
name: "buy-lounge-day-pass"
description: "Find lounges at an airport that any traveller can pay to enter, with no card or status needed."
---

# Find a lounge day pass

Find lounges at an airport that any traveller can pay to enter, with no card or status needed.

- **Skill ID:** `buy-lounge-day-pass`
- **Tags:** travel, day-pass, pay-per-use, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `find_lounges_by_access` with access_method `day-pass` and the airport's IATA code.
2. If none match, also try `priority-pass` and `loungekey` at that airport and explain that those networks sell single visits or short memberships.
3. Call `get_lounge_details` with a slug copied from the results for the best option to confirm terminal and hours. State that walk-up entry is subject to capacity.

## Tools

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

### `get_lounge_details`

Get detailed information about a specific airport lounge by its slug. Returns full description, amenities, access methods, opening hours, capacity, ratings, and reviews count.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "slug": {
      "type": "string",
      "description": "Lounge slug (from search results or airport lounges)"
    }
  },
  "required": [
    "slug"
  ],
  "type": "object"
}
```

## Example requests

- Can I just pay to get into a lounge at LAX?
- Cheapest lounge day pass at Heathrow Terminal 5

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
