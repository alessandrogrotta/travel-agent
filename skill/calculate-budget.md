---
name: calculate-budget
description: Adds up the costs of flights, accommodation, and itinerary, compares them with the total budget, and returns a green/yellow/red traffic light with cut suggestions.
---

# calculate-budget

## Goal
Check whether the planned trip fits the budget and, if needed, suggest where to save.

## Input
- `total_cost` of flights, accommodation, and itinerary (from context)
- The user's total budget
- Number of travelers

## Output
Table by category:

| Category | Cost | % of budget |
|---|---|---|
| Flights | | |
| Accommodation | | |
| Activities | | |
| Meals | | |
| Local transport / contingencies (≈10%) | | |
| **Total** | | |

Plus: **traffic light** 🟢/🟡/🔴, remaining margin (€), and cut suggestions.

## System Prompt (rules)
1. Compute the total per category and add a **contingency reserve of about 10%**.
2. Assign the **traffic light**: 🟢 green ≤ 90% of budget · 🟡 yellow 90–100% · 🔴 red > 100%.
3. If yellow or red, provide **concrete cut suggestions** with the estimated saving for each.
4. Always show the margin (or overrun) in € and %.
5. Do not modify the other skills' data: only flag discrepancies.

## Limits
- Works only on the costs received: does not look up new prices.
- Contingency and local transport figures are indicative.

## MCP
- **None** (pure calculation on the context).
