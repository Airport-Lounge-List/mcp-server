---
name: "lounge-tool-best-practice"
description: "Rules that stop the common failures: guessed slugs, missing arguments, repeated searches and rate limits."
---

# Use the lounge tools without errors

Rules that stop the common failures: guessed slugs, missing arguments, repeated searches and rate limits.

- **Skill ID:** `lounge-tool-best-practice`
- **Tags:** travel, lounges, reliability
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. When you know the airport, call `get_airport_lounges` with its IATA code. It is the most reliable tool. Use `search_lounges` only for a lounge name, city or country, with one short term per call.
2. Pass every required argument: `iata` for get_airport_lounges, `query` for search_lounges, `slug` for get_lounge_details, `access_method` for find_lounges_by_access.
3. Copy lounge slugs from a previous result. Never build a slug from a lounge name.
4. Copy access-method slugs from `list_access_methods`. Call it once per conversation and reuse the result.
5. If a search returns zero lounges, change the term once (IATA code instead of city, or the reverse), then stop and tell the user. Do not loop: search_lounges allows 60 calls an hour.
6. On a rate-limit error, stop calling that tool and use the data you already have.

## Tools

### `get_airport_lounges`

Get all airport lounges at a specific airport by IATA code. Returns every lounge at the airport with terminal location, amenities, access methods, and ratings.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "iata": {
      "type": "string",
      "pattern": "^[A-Za-z]{3}$",
      "description": "Airport IATA code (e.g. LHR, JFK, SIN)"
    }
  },
  "required": [
    "iata"
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

## Example requests

- Find the lounges in Dubai
- Get details for the lounge you just listed

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
