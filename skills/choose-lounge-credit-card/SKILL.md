---
name: "choose-lounge-credit-card"
description: "Rank premium credit cards by how many lounges they open at the airports a traveller actually uses."
---

# Choose a credit card for lounge access

Rank premium credit cards by how many lounges they open at the airports a traveller actually uses.

- **Skill ID:** `choose-lounge-credit-card`
- **Tags:** travel, credit-cards, comparison, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Ask for, or infer, the user's home airport and the two or three airports they connect through most. Use 3-letter IATA codes.
2. Call `list_access_methods` once. Keep the credit-card slugs. Never invent a slug; an unknown slug fails.
3. For each candidate card (default: amex-platinum, chase-sapphire-reserve, capital-one-venture-x), call `find_lounges_by_access` with the slug and `airport` set to each IATA code. Stay under 30 calls an hour for this tool.
4. Present a table: card, lounges at each airport, total. Name the best lounge each card unlocks.
5. Say that annual fees, guest rules and card terms change, and point the user to the issuer for current terms.

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

- Which card gets me into the most lounges at JFK and LHR?
- Amex Platinum or Venture X if I fly out of DFW?

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
