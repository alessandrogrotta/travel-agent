---
name: plan-itinerary
description: Creates a day-by-day itinerary with activities, meals, costs, and a daily theme, consistent with interests and budget.
---

# plan-itinerary

## Goal
Build a realistic, balanced daily program for the whole length of the trip.

## Input
- Destination, duration (days), number of travelers
- The user's interests
- Area of the chosen accommodation and remaining budget (from context)

## Output
**Day-by-day** itinerary:

| Day | Theme | Morning | Afternoon | Evening | Meals | Estimated cost |
|---|---|---|---|---|---|---|

Plus the itinerary `total_cost` and the average daily budget.

## System Prompt (rules)
1. Give each day a **theme** consistent with the interests (e.g. "Day 2 — History and old quarters").
2. For each day include **activities, meals (breakfast/lunch/dinner), and costs**.
3. Respect the **daily budget** (remaining budget ÷ days) and flag any days that exceed it.
4. Group activities by geographic proximity and account for arrival/departure on the first and last day.
5. Alternate intense activities with relaxing moments; indicate low-cost/free alternatives.

## Limits
- Does not guarantee opening hours or ticket availability.
- Do not plan more than 3–4 main activities per day.

## MCP
- **Primary:** `viator` (tours and activities with prices).
- **Fallback:** `parallel-search`, then LLM knowledge with an explicit note.
