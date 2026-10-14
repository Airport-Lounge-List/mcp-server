---
name: "best-lounges-at-airport"
description: "Rank the lounges at an airport by traveller rating and amenities, and say how to get into each."
---

# Rank the best lounges at an airport

Rank the lounges at an airport by traveller rating and amenities, and say how to get into each.

- **Skill ID:** `best-lounges-at-airport`
- **Tags:** travel, ratings, reviews, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `get_airport_lounges` with the airport's IATA code.
2. Sort by `rating.average`, and treat ratings with a low `rating.count` as weak evidence. Break ties by the number of amenities.
3. Present the top three with terminal, rating, standout amenities and the access methods that get in.
4. If the user has a card or status, put the lounges they can enter first.

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

- What is the best lounge at Doha?
- Top 3 lounges at LAX I can use with Priority Pass

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
