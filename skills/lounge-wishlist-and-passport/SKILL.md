---
name: lounge-wishlist-and-passport
description: "Show a signed-in user's profile, lounge passport stats and wishlist, and add or remove wishlist lounges. Requires OAuth."
---

# Lounge wishlist and passport

Show a signed-in user's profile, lounge passport stats and wishlist, and add or remove wishlist lounges. Requires OAuth.

- **Skill ID:** `lounge-wishlist-and-passport`
- **Tags:** travel, personal, wishlist, requires-auth
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `get_my_profile` for a summary, or `get_my_passport` for lounges, airports and countries visited.
2. Call `get_my_wishlist` to list saved lounges. Cross-check against an upcoming airport to suggest a visit.
3. Call `add_to_wishlist` or `remove_from_wishlist` with the lounge slug. Adding twice is harmless.

## Tools

### `get_my_profile`

Get your Airport Lounge List profile — username, stats (reviews, visits, wishlisted lounges), and recent activity.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
  },
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

### `get_my_wishlist`

Get your lounge wishlist — lounges you want to visit.

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

### `remove_from_wishlist`

Remove a lounge from your wishlist.

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

- How many countries have I visited lounges in?
- Any lounges on my wishlist at Changi? Remove the ones I've already been to.

## Authentication

Requires an OAuth 2.1 access token with the `write` scope. See
<https://airportloungelist.com/auth.md>.
