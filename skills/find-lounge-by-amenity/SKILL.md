---
name: "find-lounge-by-amenity"
description: "Find lounges at an airport that have a specific amenity, such as showers, sleeping pods, nap rooms, a spa or a quiet zone."
---

# Find a lounge with showers, sleep pods or a spa

Find lounges at an airport that have a specific amenity, such as showers, sleeping pods, nap rooms, a spa or a quiet zone.

- **Skill ID:** `find-lounge-by-amenity`
- **Tags:** travel, amenities, showers, sleep, lounges
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `get_airport_lounges` with the airport's IATA code.
2. Keep the lounges whose `amenities` contain the requested amenity. Match loosely: sleep pods, nap rooms and rest areas are the same need.
3. Call `get_lounge_details` with a slug from the results for the best match to confirm hours and access.
4. If none match, say so and list the lounges with the closest amenity.

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

- Which lounges at Istanbul have sleeping pods?
- I need a shower at Frankfurt before my connection

## Authentication

Requires an OAuth 2.1 access token with the `read` scope. See
<https://airportloungelist.com/auth.md>.
