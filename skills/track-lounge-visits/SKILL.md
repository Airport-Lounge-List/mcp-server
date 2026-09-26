---
name: track-lounge-visits
description: "Log a lounge visit, write or edit a review, and manage a wishlist on behalf of a signed-in user. Requires OAuth with the write scope."
---

# Track lounge visits and reviews

Log a lounge visit, write or edit a review, and manage a wishlist on behalf of a signed-in user. Requires OAuth with the write scope.

- **Skill ID:** `track-lounge-visits`
- **Tags:** travel, personal, reviews, requires-auth
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Find the lounge slug with `search_lounges` or `get_airport_lounges`, or pass lounge_name plus airport_iata.
2. Call `mark_lounge_visited` with the date (YYYY-MM-DD) and an optional 1-5 rating and notes.
3. If the user shares an opinion of 10 or more characters, offer to save it with `write_review`.
4. Call `get_my_passport` to show the updated stats.

## Tools

### `mark_lounge_visited`

Log a lounge visit (check in). Identify the lounge by its slug OR by lounge_name + airport_iata. Optionally add a 1-5 rating and notes. Idempotent: logging the same lounge on the same date updates the existing visit's rating/notes instead of creating a duplicate.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug (from search results). Preferred if known."
    },
    "lounge_name": {
      "type": "string",
      "description": "Lounge name — use with airport_iata if you don't have the slug."
    },
    "airport_iata": {
      "type": "string",
      "description": "3-letter airport IATA code — use with lounge_name."
    },
    "visited_at": {
      "type": "string",
      "format": "date",
      "description": "Date of visit (YYYY-MM-DD)"
    },
    "rating": {
      "type": "integer",
      "description": "Your rating, 1 (poor) to 5 (excellent)",
      "minimum": 1,
      "maximum": 5
    },
    "notes": {
      "type": "string",
      "description": "Optional notes about your visit"
    }
  },
  "required": [
    "visited_at"
  ],
  "type": "object"
}
```

### `write_review`

Write a review for a lounge. Requires the lounge slug, a rating (1-5), and review text (min 10 characters).

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug (from search results)"
    },
    "rating": {
      "type": "integer",
      "description": "Rating from 1 (poor) to 5 (excellent)",
      "minimum": 1,
      "maximum": 5
    },
    "content": {
      "type": "string",
      "description": "Review text (minimum 10 characters)",
      "minLength": 10
    }
  },
  "required": [
    "lounge_slug",
    "rating",
    "content"
  ],
  "type": "object"
}
```

### `get_my_passport`

Get your lounge passport — stats (lounges visited, airports, countries, reviews), country stamps, full visit history, and your reviews. Includes a shareable passport URL when your profile is public.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
  },
  "type": "object"
}
```

### `add_to_wishlist`

Add a lounge to your wishlist. Idempotent — adding a lounge already on your wishlist is a no-op.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug (from search results)"
    }
  },
  "required": [
    "lounge_slug"
  ],
  "type": "object"
}
```

## Example requests

- Log that I visited the Qantas First Lounge in Sydney yesterday
- Add the Cathay Pacific Pier to my lounge wishlist

## Authentication

Requires an OAuth 2.1 access token with the `write` scope. See
<https://airportloungelist.com/auth.md>.
