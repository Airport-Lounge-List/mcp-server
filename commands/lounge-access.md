---
description: Check which lounges a card, membership or status gets you into
argument-hint: "[access method] [airport IATA]"
allowed-tools:
  - mcp__airport-lounge-list__list_access_methods
  - mcp__airport-lounge-list__find_lounges_by_access
  - mcp__airport-lounge-list__get_lounge_details
---

# Lounge Access

The user wants to know which lounges they can get into.

## Arguments

The user invoked this command with: $ARGUMENTS

Parse the arguments to extract:
- **Access method**: a credit card, membership, airline status or ticket class (e.g. Amex Platinum, Priority Pass, oneworld Sapphire, business class)
- **Airport**: optional IATA code

If no access method is given, ask what card, membership or status they have.

## Process

1. Call `list_access_methods` and match the user's words to an access method slug.
2. Call `find_lounges_by_access` with that slug, and the airport if given.
3. Present the lounges grouped by airport: Lounge | Terminal | Notes.
4. Say any guest rules or limits from the access method description.
