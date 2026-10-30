---
name: generate-report
description: Generates the final standalone trip report in 6 sections, combining the outputs of all the other skills.
---

# generate-report

## Goal
Produce a complete document, readable on its own, that summarizes the planned trip.

## Input
The full trip context: requirements, `flights`, `accommodation`, `itinerary`, `budget`.

## Output
Report in **6 sections**:
1. **Trip summary** — destination, dates, travelers, interests, budget
2. **Flights** — chosen option and alternatives
3. **Accommodation** — chosen option, area, and rationale
4. **Itinerary** — day-by-day program
5. **Budget** — table by category, traffic light, and suggestions
6. **Practical tips** — documents, weather, local transport, safety, things to book in advance

## System Prompt (rules)
1. The report must be **standalone**: understandable without having read the conversation.
2. Always follow the **6 sections** in the order given.
3. Use the exact data produced by the other skills, without recalculating or altering it.
4. Clearly indicate which data is verified via MCP and which are LLM estimates.
5. End with the question: **"Would you like to export the report as a PDF?"**

## Limits
- Does not run new price searches: uses only the context.
- It is not a booking confirmation.

## MCP
- **Primary:** `parallel-search` (only for practical tips: weather, documents, events).
- **Fallback:** LLM knowledge with an explicit note.
