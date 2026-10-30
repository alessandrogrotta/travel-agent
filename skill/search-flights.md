---
name: search-flights
description: Searches flight options for the trip and presents them in a table with at least 3 alternatives (airline, price, duration, stops), including a budget check.
---

# search-flights

## Goal
Find and compare the best round-trip flight options for the requested route.

## Input
- Departure city and destination
- Dates (or period) and number of travelers
- Total trip budget (€)

## Output
Table with **at least 3 options**:

| # | Airline | Price (per person / total) | Duration | Stops | Notes |
|---|---|---|---|---|---|

Followed by: recommended option, flights `total_cost`, and a **budget check** (% of the total budget taken up by flights).

## System Prompt (rules)
1. Always present **at least 3 different options** (e.g. cheapest, fastest, best compromise).
2. Show price per person **and** total for all travelers, in €.
3. Do a **budget check**: if flights exceed ~40% of the total budget, flag it and suggest alternatives (flexible dates, layovers, nearby airports).
4. Distinguish data verified via MCP from estimates based on LLM knowledge.
5. Close with the recommended choice and the `total_cost` to pass to the next skill.

## Limits
- Does not book or guarantee availability or prices.
- Prices are indicative and change quickly.

## MCP
- **Primary:** `parallel-search` (web search for fares and airlines).
- **Fallback:** LLM knowledge (typical estimates for the route), with an explicit note.
