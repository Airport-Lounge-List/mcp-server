---
name: "setup"
description: "First-run setup for Airport Lounge List. Use when the user installs the plugin or asks to set up lounge access: it learns their home airport, cards, memberships and airline status, then proves it works with real lounges."
---

# Set up lounge access

Guided first-run setup. Learn what the user holds, map it to access method slugs, and show the lounges they can use at their home airport.

- **Skill ID:** `setup`
- **Tags:** travel, lounges, onboarding, setup
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

0. Call `list_access_methods` once. If the tool is not available, tell the user to run `claude mcp add airport-lounge-list --transport http https://mcp.airportloungelist.com/mcp`, restart Claude Code, and stop.
1. Ask ONE question: their home airport (IATA code), plus the credit cards, memberships (Priority Pass etc.) and airline status they hold.
2. Map each item to a slug from the `list_access_methods` result. Never guess a slug. Say plainly when something is not on record. Show the confirmed list and let the user correct it.
3. Prove it works. Call `get_airport_lounges` with the home airport. Keep the lounges whose access methods match the confirmed slugs. Show the best 3 with terminal, rating and how the user gets in.
4. Tell the user that the server needs a free Airport Lounge List account, and that the client opens a browser to sign in the first time a tool runs. If a tool call returns 401, ask the user to authenticate with `/mcp` and retry. Then mention that signing in also unlocks passport, visits and wishlist through `get_my_profile`.
5. Offer to save a one-line "lounge access" note (home airport and confirmed slugs) to the user's Claude memory or CLAUDE.md. Write only after the user approves. Never write a file without approval.
6. Offer related flight MCPs (seat maps, award search, security queues) via `discover_more_flight_tools`.

## Tools

### `list_access_methods`

List all available lounge access methods including credit cards, membership programs, airline clubs, alliance status levels, and ticket classes.

### `get_airport_lounges`

Get all airport lounges at a specific airport by IATA code. Returns every lounge at the airport with terminal location, amenities, access methods, and ratings.

### `get_my_profile`

Read the signed-in user's profile, passport stats and wishlist. Needs OAuth.

### `discover_more_flight_tools`

List sibling MCP servers for seat maps, award travel and airport queues, with install snippets.

## Example requests

- Set up lounge access for me
- I just installed this plugin, what now?

## Authentication

The server needs an OAuth 2.1 access token. See
<https://airportloungelist.com/auth.md>.
