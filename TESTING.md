# Testing

Use these prompts to check the plugin end to end. The first tool call opens a browser to sign in with a free
account. Every prompt below only reads data, so nothing is changed on the account.

## 1. Lounges at an airport

> Which lounges are at London Heathrow Terminal 5?

Expected tools: `get_airport_lounges`, then `get_lounge_details` for one result. The answer lists named lounges with
terminal, opening hours and access rules.

## 2. Access by card or status

> What access methods can I filter by, and which lounges in Singapore can I enter with business class?

Expected tools: `list_access_methods`, then `find_lounges_by_access` with a slug returned by the first call. The
slug comes from the tool output, not from a guess.

## 3. Amenities

> Find lounges at Dubai International with showers.

Expected tools: `search_airports` to resolve the airport, then `find_lounges_by_amenity`.

## 4. Crowding

> When is the quietest time to visit the lounges at Frankfurt today?

Expected tools: `search_lounges`, then `get_lounge_busyness` for the chosen lounge. The answer gives the busiest and
quietest hours in the lounge's local time.
