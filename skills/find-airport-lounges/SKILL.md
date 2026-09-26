---
name: find-airport-lounges
description: "List every lounge at an airport or in a city, with terminal, hours, amenities and how to get in."
---

# Find airport lounges

List every lounge at an airport or in a city, with terminal, hours, amenities and how to get in.

- **Skill ID:** `find-airport-lounges`
- **Tags:** travel, airports, lounges, search
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. If the user gives an airport, call `get_airport_lounges` with its 3-letter IATA code. Otherwise call `search_lounges` with one concept only (a city, airport or lounge name), not a combined phrase.
2. Filter the results by terminal or amenity (showers, spa, sleeping pods) in your answer.
3. Call `get_lounge_details` for the one or two lounges the user cares about to get hours and the full access list.

## Tools

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

- What lounges are at SIN Terminal 3?
- Show me the lounges at Heathrow with showers

## Authentication

None. Call the endpoint anonymously.
