---
name: travel-agent
description: Trip planning orchestrator. Collects requirements and coordinates the 5 sub-skills in a fixed order (flights → accommodation → itinerary → budget → report). Use it when the user wants to plan a complete trip.
---

# Travel Agent — Orchestrator

## Goal
Plan a complete trip by orchestrating the 5 sub-skills in `skill/`. Each output becomes the input of the next skill.

## Phase 0 — Requirements gathering (MANDATORY before starting)
Do not start any skill until you have all of the following. If anything is missing, ask the user for it in a single message:

| Requirement | Example |
|---|---|
| Destination (and departure city) | Lisbon, from Turin |
| Total budget (€) | €1200 |
| Dates / duration | 5 days, March 12–17 |
| Interests | food, beaches, history |
| Travelers | 2 people |

Summarize the requirements and ask for confirmation before proceeding.

## Orchestrator rules
1. **FIXED execution order:** `search-flights` → `search-accommodation` → `plan-itinerary` → `calculate-budget` → `generate-report`. Never skip or reorder steps.
2. **Each output becomes the input of the next skill.** Keep and pass the *trip context* (see below): the flight cost goes to `calculate-budget`, the itinerary goes to `generate-report`.
3. **MCP fallback:** if an MCP does not respond (connection error, 403, timeout), move on to the next server; if none works, use LLM knowledge and flag it with a note ("estimates based on general knowledge, not verified in real time"). **Never block the flow.**
4. **Show intermediate results** to the user after each skill, with a short phase title (e.g. "Phase 1/5 — Flights").
5. **At the end always ask:** *"Would you like to export the report as a PDF?"* If yes, run `skill/export-pdf.md`, passing the full report and trip context.

## Trip context (to pass between skills)
```yaml
requirements:
  departure: ...
  destination: ...
  dates: ...
  duration_days: ...
  total_budget: ...
  interests: [...]
  travelers: ...
flights:          # output of search-flights
  chosen_option: { airline, price, duration, stops }
  total_cost: ...
accommodation:    # output of search-accommodation
  chosen_option: { name, price_per_night, rating, area }
  total_cost: ...
itinerary:        # output of plan-itinerary
  days: [ { day, theme, activities, meals, cost } ]
  total_cost: ...
budget:           # output of calculate-budget
  categories: [...]
  traffic_light: green | yellow | red
  suggestions: [...]
```

## Execution flow
1. **Requirements** → collect and confirm.
2. **`skill/search-flights.md`** → input: departure, destination, dates, travelers, budget. Save `flights`.
3. **`skill/search-accommodation.md`** → input: destination, dates, travelers, remaining budget after flights. Save `accommodation`.
4. **`skill/plan-itinerary.md`** → input: destination, duration, interests, remaining budget after flights and accommodation. Save `itinerary`.
5. **`skill/calculate-budget.md`** → input: flight + accommodation + itinerary costs and the total budget. Save `budget`.
   - If the traffic light is **red**, propose the suggested cuts and ask the user whether to re-run the affected phases.
6. **`skill/generate-report.md`** → input: the full context. Produces the final 6-section report.
7. **Closing:** ask *"Would you like to export the report as a PDF?"* If yes, run **`skill/export-pdf.md`** with the full report and trip context, then tell the user where the file was saved.

## Limits
- Do not book anything: provide only proposals and estimates.
- Do not present prices as verified when they are not: always distinguish MCP data from LLM estimates.
- If the requirements change mid-flow, restart from the first affected skill.

## MCPs used (through the sub-skills)
`parallel-search` (flights, report) · `gondola` (accommodation) · `viator` (activities). All optional.
