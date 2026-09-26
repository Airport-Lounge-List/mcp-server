---
name: manage-lounge-history
description: "Review, correct or delete a signed-in user's logged visits and reviews. Requires OAuth with the write scope."
---

# Edit lounge visits and reviews

Review, correct or delete a signed-in user's logged visits and reviews. Requires OAuth with the write scope.

- **Skill ID:** `manage-lounge-history`
- **Tags:** travel, personal, reviews, requires-auth
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Call `get_my_visits` or `get_my_reviews` to find the entry and its lounge slug.
2. To change it, call `update_visit` or `update_review`. Pass visited_at only when the user has several visits to the same lounge.
3. To remove it, confirm with the user first, then call `delete_visit` or `delete_review`. Deletes are permanent.

## Tools

### `get_my_visits`

Get your visited lounges list with dates and notes.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "limit": {
      "type": "integer",
      "description": "Max visits to return (default 20, max 50)"
    }
  },
  "type": "object"
}
```

### `update_visit`

Update one of your logged lounge visits — change the rating, notes, or date. Identify the visit by lounge_slug; if you have multiple visits to the same lounge, pass visited_at to pick the one to update.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug of the visit to update"
    },
    "visited_at": {
      "type": "string",
      "format": "date",
      "description": "Existing visit date (YYYY-MM-DD) — needed only to disambiguate multiple visits"
    },
    "rating": {
      "type": "integer",
      "description": "New rating, 1-5",
      "minimum": 1,
      "maximum": 5
    },
    "notes": {
      "type": "string",
      "description": "New notes"
    },
    "new_visited_at": {
      "type": "string",
      "format": "date",
      "description": "Change the visit date to this (YYYY-MM-DD)"
    }
  },
  "required": [
    "lounge_slug"
  ],
  "type": "object"
}
```

### `delete_visit`

Delete one of your logged lounge visits. Identify it by lounge_slug; pass visited_at to pick a specific date if you have multiple visits to the same lounge.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug of the visit to delete"
    },
    "visited_at": {
      "type": "string",
      "format": "date",
      "description": "Visit date (YYYY-MM-DD) — needed only to disambiguate multiple visits"
    }
  },
  "required": [
    "lounge_slug"
  ],
  "type": "object"
}
```

### `get_my_reviews`

Get your lounge reviews, including pending ones. Returns rating, content, approval status, and lounge info.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "limit": {
      "type": "integer",
      "description": "Max reviews to return (default 20, max 50)"
    }
  },
  "type": "object"
}
```

### `update_review`

Update your review of a lounge — change the rating and/or the text. Identify the review by lounge_slug.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug of the review to update"
    },
    "rating": {
      "type": "integer",
      "description": "New rating, 1-5",
      "minimum": 1,
      "maximum": 5
    },
    "content": {
      "type": "string",
      "description": "New review text (minimum 10 characters)",
      "minLength": 10
    }
  },
  "required": [
    "lounge_slug"
  ],
  "type": "object"
}
```

### `delete_review`

Delete your review of a lounge. Identify it by lounge_slug.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
    "lounge_slug": {
      "type": "string",
      "description": "Lounge slug of the review to delete"
    }
  },
  "required": [
    "lounge_slug"
  ],
  "type": "object"
}
```

## Example requests

- Change my rating of the Heathrow Concorde Room visit to 5 stars
- Delete the review I wrote for the Plaza Premium lounge in KUL

## Authentication

Requires an OAuth 2.1 access token with the `write` scope. See
<https://airportloungelist.com/auth.md>.
