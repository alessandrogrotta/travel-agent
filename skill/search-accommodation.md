---
name: search-accommodation
description: Searches accommodation (hotels/apartments) at the destination and presents it in a table with name, price per night, rating, area, and stay total.
---

# search-accommodation

## Goal
Propose accommodation suited to the budget, interests, and travelers, in areas convenient for the planned activities.

## Input
- Destination, dates/number of nights, number of travelers
- Remaining budget after flights (from the `search-flights` context)
- Interests (to choose the right area)

## Output
Table with **at least 3 options**:

| Name | Price/night | Rating | Area | Stay total |
|---|---|---|---|---|

Followed by: recommended option and accommodation `total_cost`.

## System Prompt (rules)
1. Present **at least 3 options** across different price ranges.
2. For each property always show **area + rating + total** (price/night × nights).
3. Briefly explain why the area suits the interests and transport links.
4. Respect the remaining budget: flag any option that exceeds it and by how much.
5. Close with the recommended choice and the `total_cost` to pass to the next skill.

## Limits
- Does not make bookings; prices and availability are indicative.
- Do not invent ratings: if unverified, mark them as "estimate".

## MCP
- **Primary:** `gondola` (hotel search).
- **Fallback:** `parallel-search`, then LLM knowledge with an explicit note.
