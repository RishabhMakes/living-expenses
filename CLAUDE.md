# Living Expenses Calculator — Codebase Notes

## Overview

Single-file HTML app (`index.html`) — no build step, no dependencies. Opens directly in a browser. All HTML, CSS, and JavaScript live in one ~37 KB file.

Calculates the minimum gross household income needed to sustain a given lifestyle across 5 Indian metros: Mumbai, Delhi NCR, Bengaluru, Hyderabad, Pune (in that order — all city arrays follow this index).

---

## Key Constants & Data Structures

### City order (index 0–4)
```
MUM  DEL  BLR  HYD  PNE
```
Every array of length 5 follows this order.

### `N_CITIES = 5`

### `STATIC_SECTIONS`
Fixed expense sections (Housing, Transport, Domestic Help, Food, Health, Lifestyle, Misc). Each row has:
- `vals` — premium-tier values
- `midVals` — mid-neighbourhood values (optional, falls back to `vals`)
- `normalVals` — normal-neighbourhood values (optional, falls back to `vals`)

### `PER_KID`
Per-child monthly costs multiplied by `k` in `buildSections`:
- `school`, `tutoring`, `extracurricular`, `books`, `collegeSIP`

### `INTL_TRAVEL_TIERS`
International travel tiers keyed by `'luxury' | 'highend' | 'normal'`:
- `label` — quality description shown in the UI hint
- `monthly[5]` — base monthly cost for 2 adults (0 kids), identical across cities
- `perKidMonthly` — flat increment per child (same for all cities, since flights go to the same destination)

Increments: luxury ₹20K, highend ₹15K, normal ₹10K per child/month.

### `DOMESTIC_TRAVEL_BASE[5]` / `DOMESTIC_TRAVEL_PER_KID[5]`
Base domestic travel (couple, 0 kids) and per-child increment, both city-specific.

### `NEIGHBOURHOOD_TIERS`
Three tiers (`premium`, `mid`, `normal`) with display names per city. Used to resolve `STATIC_SECTIONS` rows and update the neighbourhood header row.

### `COLLEGE_DESTINATIONS`
Keyed by country code. Each entry has `name`, `flag`, `sip` (monthly SIP per child), `basis` (hint text).

---

## Core Functions

### `buildSections(kids, travelTier, collegeDest, tier)`
Builds the full list of expense sections for the current state. Called on every render.

- Resolves `STATIC_SECTIONS` rows to the correct neighbourhood tier (`r[tier+'Vals'] ?? r.vals`).
- Education and College SIP sections only appear when `kids > 0`.
- Travel rows scale with kids:
  - `intlMonthly = base + k * perKidMonthly`
  - `domesticMonthly = DOMESTIC_TRAVEL_BASE + k * DOMESTIC_TRAVEL_PER_KID`
- Travel row label: `"Couple"` when 0 kids, `"Family of N"` otherwise.

### `render()`
Called on every state change. Rebuilds the entire `<tbody>`, then updates:
- Monthly/annual totals
- Tax calculations (gross income solver)
- Travel hint text (dynamically reflects current kids + tier → annual ₹ L/yr)

### `findGrossForPostTax(postTax)`
Binary search (256 iterations) to find the gross income that yields the required post-tax amount under the Indian New Tax Regime FY 2024-25. Accounts for progressive slabs, surcharge, marginal relief, and 4% cess.

### `computeTax(gross)` / `taxBreakdown(gross)`
New Tax Regime slab-based calculations with surcharge thresholds at ₹50L, ₹1Cr, ₹2Cr, ₹5Cr.

---

## State

```javascript
const state = { kids: 2, travelTier: 'highend', collegeDest: 'usa', neighbourTier: 'premium' };
```

All controls update `state` and call `render()`.

---

## UI Controls

| Control | ID | Values |
|---|---|---|
| Neighbourhood tier | `neighbour-tier` | `premium / mid / normal` |
| Number of kids | `.kid-btn[data-kids]` | `0 – 4` |
| College destination | `college-dest` | `usa / uk / canada / australia / singapore / india` |
| International travel tier | `travel-tier` | `luxury / highend / normal` |

---

## What Scales with Kids

| Expense | Scales? |
|---|---|
| School fees, tutoring, extracurriculars, books | Yes — `× k` |
| College SIP | Yes — `× k` |
| International travel | Yes — `base + k × perKidMonthly` |
| Domestic travel | Yes — `base + k × perKidPerCity` |
| Housing, transport, food, health, lifestyle, misc | No |
