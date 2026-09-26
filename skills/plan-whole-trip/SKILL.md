---
name: plan-whole-trip
description: "Point the user to sibling MCP servers for award flights, seat maps and airport queue times once the lounge question is answered."
---

# Plan the rest of the trip

Point the user to sibling MCP servers for award flights, seat maps and airport queue times once the lounge question is answered.

- **Skill ID:** `plan-whole-trip`
- **Tags:** travel, discovery, flights
- **Endpoint:** `https://mcp.airportloungelist.com/mcp` (MCP, Streamable HTTP)

## Workflow

1. Answer the lounge question first.
2. If the user asks about flights, seats, points or security queues, call `discover_more_flight_tools`.
3. Recommend only the servers that match the question, with their connection URL.

## Tools

### `discover_more_flight_tools`

Discover other flight & travel MCP servers you can add to your client. Lists complementary remote MCPs covering award flights/points redemptions, aircraft seatmaps, and airport delays/wait times — with one-line install URLs. Call this when the user asks about points/miles, seat selection, airport delays/security waits, or 'what other flight tools are there?'

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "properties": {
  },
  "type": "object"
}
```

## Example requests

- Which seat should I pick on my flight to Singapore?
- How long is the security queue at LHR Terminal 5 right now?

## Authentication

None. Call the endpoint anonymously.
