---
name: plan-layover-lounges
description: "Given an itinerary and the cards or memberships a traveller holds, find the best lounge they can enter at each departure and connection airport."
---

# Plan lounges for a trip or layover

Given an itinerary and the cards or memberships a traveller holds, find the best lounge they can enter at each departure and connection airport.

- **Skill ID:** `plan-layover-lounges`
- **Tags:** travel, itinerary, layover, credit-cards
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. List the departure and connection airports as IATA codes. Skip the final arrival airport.
2. Call `list_access_methods` once to map every card, membership, status and ticket class to slugs.
3. For each airport, call `get_airport_lounges` and keep the lounges whose access methods match any slug.
4. Rank the matches by rating and amenities, and prefer the user's departure terminal. Call `get_lounge_details` on the top pick to confirm opening hours cover the layover.
5. Answer with one recommendation per airport plus a fallback. Say so when no lounge matches.

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

- I fly LHR to SIN to SYD in business on Singapore Airlines with a Priority Pass. Where can I go?
- 4-hour layover in DOH with an Amex Platinum — best lounge?

## Authentication

None. Call the endpoint anonymously.
