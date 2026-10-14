---
name: "does-my-card-get-lounge-access"
description: "Answer yes or no for one card, membership or airline status, and list what it does unlock."
---

# Does my card or status get lounge access?

Answer yes or no for one card, membership or airline status, and list what it does unlock.

- **Skill ID:** `does-my-card-get-lounge-access`
- **Tags:** travel, credit-cards, eligibility
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `list_access_methods` and match the user's card or status to a slug by name.
2. If nothing matches, answer that we have no lounge access on record for it. Do not guess a near slug. Suggest the closest cards from the list that do include access.
3. If it matches, describe the access in one sentence from the method's description, then call `find_lounges_by_access` with an airport the user named to give concrete examples.

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

- Does Chase Sapphire Preferred have lounge access?
- Can I get into a lounge with Amex Gold?

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
