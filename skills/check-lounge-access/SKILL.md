---
name: check-lounge-access
description: "Work out whether a specific credit card, membership, airline status or day pass gets you into a given lounge."
---

# Check lounge access eligibility

Work out whether a specific credit card, membership, airline status or day pass gets you into a given lounge.

- **Skill ID:** `check-lounge-access`
- **Tags:** travel, credit-cards, memberships, eligibility
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `list_access_methods` to map the user's card, membership or status to an access method slug.
2. For one lounge, call `get_lounge_details` and check its access methods. For an airport, call `find_lounges_by_access` with the slug and keep the lounges at that airport.
3. Say clearly which lounges are in and which are out, and note guest rules or fees when the data has them.

## Tools

### `find_lounges_by_access`

Find all airport lounges accessible with a specific credit card, membership program, airline club, alliance status, or ticket class. For example, find all lounges you can access with your Amex Platinum or Priority Pass membership.

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
        "priority-pass",
        "loungekey",
        "dragonpass",
        "diners-club",
        "plaza-premium",
        "business-class",
        "first-class",
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
    }
  },
  "required": [
    "access_method"
  ],
  "type": "object"
}
```

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

- Does my Amex Platinum get me into the Centurion Lounge at LAX?
- Which lounges at JFK take Priority Pass?

## Authentication

None. Call the endpoint anonymously.
