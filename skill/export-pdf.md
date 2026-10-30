---
name: export-pdf
description: Exports the complete trip report (all 6 sections plus the trip context) into a polished, professionally designed, size-optimized A4 PDF with cover page, table of contents, styled tables, day cards, budget chart, and traffic-light badge.
---

# export-pdf

## Goal
Turn the full result of the research into a clean, print-ready PDF that looks professional, is easy to scan, and stays lightweight.

## Input
- The final report from `generate-report` (6 sections) and the full trip context (`requirements`, `flights`, `accommodation`, `itinerary`, `budget`)
- Language of the report (the PDF uses the same language)
- Optional: custom file name

## Output
- A single **A4 PDF**: `travel-plan_<destination>_<YYYY-MM-DD>.pdf`
- A short confirmation message: file path, page count, file size

## System Prompt (rules)
1. **Export everything, change nothing.** Include all 6 sections and every figure exactly as produced by the other skills. Never recalculate, shorten, or reword data.
2. **Use the fixed document structure:** cover → at-a-glance summary → table of contents → the 6 report sections in order → closing page with disclaimer.
3. **Apply the design specification below** consistently (palette, typography, tables, headers/footers). The result must look like a professional travel proposal, not a text dump.
4. **Optimize the file:** vector graphics only, standard or subset-embedded fonts, no heavy images, compressed streams. Target **under 1 MB** for a typical trip.
5. **Verify before delivering:** check page count, text overflow, broken tables, missing characters (€, accents), and orphan headings. Fix and regenerate if anything is off.

## Design specification

### Page and typography
| Element | Specification |
|---|---|
| Format | A4 portrait, margins 20 mm (top 22 mm, bottom 20 mm) |
| Font | Clean sans-serif. Prefer an embedded Unicode font (DejaVu Sans / Noto Sans) so `€` and accented letters render correctly; otherwise Helvetica |
| Cover title | 30 pt bold |
| Section title (H1) | 18 pt bold, navy |
| Subsection (H2) | 13 pt bold |
| Body | 10 pt, line height 14 pt, dark grey |
| Tables | 9 pt |
| Footer / notes | 8 pt, muted grey |

### Color palette
| Role | Color |
|---|---|
| Primary (headers, cover band) | Navy `#1A237E` |
| Accent (rules, highlights) | Blue `#42A5F5` |
| Secondary accent | Orange `#FF7043` |
| Text | `#212121` |
| Muted text | `#757575` |
| Light background / zebra rows | `#F4F4F8` |
| Traffic light | Green `#66BB6A` · Yellow `#FFCA28` · Red `#EF5350` |

### Pages
1. **Cover:** full-width navy band, large title ("Trip Plan"), destination in large type, subtitle with dates, travelers, and total budget, blue accent line, generation date, and "Prepared by AI Travel Agent".
2. **At a glance:** four KPI cards (Total budget · Estimated total cost · Remaining margin · Traffic light) plus the key choices (chosen flight, chosen accommodation, trip duration, interests).
3. **Table of contents:** section titles with page numbers and clickable links; also add PDF bookmarks (outline).
4. **Sections 1–6:** each starts with a numbered badge, title, and blue underline.
5. **Closing page:** disclaimer (prices are indicative, not a booking confirmation), data provenance note, generation date.

### Components
- **Tables:** navy header row with white bold text, zebra rows, thin light-grey horizontal rules, numbers right-aligned, header row **repeated on every page** if the table breaks. Highlight the recommended option with a light-blue row and a "Recommended" tag.
- **Itinerary:** one **card per day** with a colored left bar, "Day N — Theme", morning/afternoon/evening slots, meals line, and the day's cost aligned right. Keep each day card together on one page.
- **Budget:** category table, a **horizontal stacked bar** (vector) showing each category's share of the budget, the remaining margin, and a **traffic-light badge** drawn as a colored circle with the label GREEN / YELLOW / RED. Cut suggestions appear as a bulleted list with estimated savings.
- **Data provenance:** mark each figure as *verified via MCP* or *LLM estimate* with a small tag or footnote marker, and explain the legend once on the closing page.
- **Practical tips:** short checklist cards (documents, weather, transport, safety, bookings).
- **Header and footer (all pages except the cover):** running header with trip name; footer with a thin rule and page numbers as `n / N`.
- **Metadata:** set PDF title, author ("AI Travel Agent"), subject, and keywords.

### Text rules
- **Never print emoji** in the PDF (most PDF fonts cannot render them). Replace with drawn shapes, colored badges, or plain text. Example: 🟢 → green circle + "GREEN".
- Format money consistently (e.g. `€1,200` or `1.200 €`, matching the report language) and align decimals.
- Avoid orphan headings (a title at the bottom of a page) and avoid splitting short tables.

## Generation method (in order of preference)
1. **Installed PDF skill or tool**, if available.
2. **Python + `reportlab`** (Platypus): best control over layout, vector charts, small file size.
3. **HTML + CSS → PDF** (`weasyprint` or headless Chromium) using `@page { size: A4; margin: 20mm }` and print CSS.
4. **Pandoc** with a styled template.
5. **If no PDF tool is available:** do not block. Produce a print-ready HTML (or Markdown) file with the same styling and tell the user how to print it to PDF.

## Quality checklist (run before delivering)
- [ ] All 6 sections present, in order, with unchanged figures
- [ ] Cover, at-a-glance page, and table of contents present
- [ ] No text overflow, clipped tables, or empty pages
- [ ] `€` and accented characters render correctly; no emoji or placeholder squares
- [ ] Traffic light, KPI cards, and budget chart match the `calculate-budget` output
- [ ] Page numbers and header/footer correct
- [ ] File size under ~1 MB (compress or simplify if larger)

## Limits
- Does not fetch new data or modify prices, only formats what it receives.
- Requires a PDF-capable tool in the environment (see fallback chain above).
- Includes images only if the user provides them; the default design is text and vector based.
- The PDF is a planning document, not a booking confirmation.

## MCP
- **None** (document generation only).
