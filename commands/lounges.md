---
description: Find the lounges at an airport, near a place, or with an amenity
argument-hint: "[airport IATA or place] [amenity...]"
---

# Find Lounges

The user wants to find airport lounges.

## Arguments

The user invoked this command with: $ARGUMENTS

Parse the arguments to extract:
- **Airport**: an IATA code (e.g. LHR, JFK, SIN) or an airport or city name
- **Amenity**: optional, one of showers, quiet-zones, family-friendly, wifi, dining, spa

If no airport is given, ask for one.

## Process

1. If the airport is not an IATA code, call `search_airports` with it and pick the match.
2. With an amenity, call `find_lounges_by_amenity` with the amenity and the IATA code. Otherwise call `get_airport_lounges`.
3. Present the lounges in a table: Lounge | Terminal | Access | Rating.
4. Offer details with `get_lounge_details`, or crowd levels with `get_lounge_busyness`.
