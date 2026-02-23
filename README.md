# 🇮🇳 Minimum Household Income: India 5-City Comparison

An interactive single-file HTML calculator that estimates the **minimum gross annual income** a household needs to sustain an upper-middle-class lifestyle across five major Indian cities — Mumbai, Delhi NCR, Bengaluru, Hyderabad, and Pune.

> **Original idea and numbers** by [@deedydas](https://x.com/deedydas) — see the source tweet:
> [https://x.com/deedydas/status/2025768860805476412](https://x.com/deedydas/status/2025768860805476412)

Available here live: [https://rishabhmakes.github.io/living-expenses/]
---

## What It Does

The calculator builds a detailed monthly expense budget across housing, lifestyle, children, and savings — then works *backwards* through India's New Tax Regime (FY 2024–25) to find the **gross pre-tax income** that covers it all. Every number updates live as you change the scenario controls.

---

## Scenario Assumptions

The default scenario mirrors the original tweet's assumptions:

| Parameter | Default |
|---|---|
| Household composition | 2 adults + 2 kids |
| Housing | 4-bedroom home (owned, with EMI) |
| Schools | Best private (IB / IGCSE) |
| College savings | Monthly SIP for USA university |
| International travel | 1 trip/yr, High-end (premium economy) |
| Neighbourhood | Premium tier per city |
| Tax regime | New Regime, FY 2024–25 |

---

## Scenario Controls

All four knobs update the table instantly:

| Control | Options |
|---|---|
| **Neighbourhood Tier** | 🏙️ Premium · 🏘️ Mid-neighbourhood · 🏡 Normal |
| **Number of Kids** | 0 · 1 · 2 · 3 · 4 |
| **College Destination** | 🇺🇸 USA · 🇬🇧 UK · 🇨🇦 Canada · 🇦🇺 Australia · 🇸🇬 Singapore · 🇮🇳 India (Top Tier) |
| **International Travel** | ✈️ Luxury (Business/First, ~₹18 L/yr) · 🌍 High-end (Premium economy, ~₹13.2 L/yr) · 🧳 Normal (Economy, ~₹7.2 L/yr) |

---

## Expense Categories Covered

**Housing & Transport**
- Home Loan EMI (4BHK, ~90% LTV)
- Home maintenance, property tax, insurance
- Car EMI (premium SUV or sedan)
- Fuel, parking, driver
- Car insurance & servicing
- Domestic help (cook, cleaner, etc.)

**Household Running Costs**
- Groceries & dining out
- Utilities (electricity, water, gas, internet, mobile)
- OTT / streaming subscriptions
- Clothing & personal care
- Health insurance (family floater)
- Out-of-pocket medical

**Children** *(scales with number of kids; hidden when 0 kids)*
- School fees (IB/IGCSE private)
- Tutoring & extracurriculars
- Books, supplies, uniforms
- College SIP (10-yr monthly SIP @ 12% CAGR to fund full degree + living costs)

**Lifestyle & Savings**
- International travel (flat across cities — uses Bengaluru benchmark)
- Domestic travel & weekend getaways
- Entertainment & hobbies
- Emergency fund SIP
- Retirement / wealth SIP

**Tax Section**
- Monthly income tax (computed from annual gross)
- Effective tax rate %
- Annual tax liability

**Bottom Lines**
- Monthly post-tax take-home needed
- Annual total spend
- Required gross annual income (pre-tax)
- Required monthly gross (CTC equivalent)

---

## Neighbourhood Tiers by City

| Tier | Mumbai | Delhi NCR | Bengaluru | Hyderabad | Pune |
|---|---|---|---|---|---|
| 🏙️ Premium | Worli / BKC / Bandra W | DLF / Vasant Vihar | Indiranagar / Whitefield | Jubilee Hills / Banjara Hills | Koregaon Pk / Kalyani Nagar |
| 🏘️ Mid | Andheri W / Powai | Noida Sec 50 / Sohna Rd | Koramangala / HSR Layout | Madhapur / Gachibowli | Viman Nagar / Baner |
| 🏡 Normal | Goregaon / Kandivali | Dwarka / Greater Noida | Marathahalli / Sarjapur Rd | Kukatpally / KPHB | Wakad / Hinjewadi |

---

## Tax Engine

Income tax is computed using **India's New Tax Regime, FY 2024–25**:

| Income Slab | Rate |
|---|---|
| Up to ₹3 L | Nil |
| ₹3 L – ₹7 L | 5% |
| ₹7 L – ₹10 L | 10% |
| ₹10 L – ₹12 L | 15% |
| ₹12 L – ₹15 L | 20% |
| Above ₹15 L | 30% |

Surcharge (on tax liability): 10% (₹50 L–1 Cr) / 15% (₹1–2 Cr) / 25% (₹2–5 Cr) / 25% capped (above ₹5 Cr). Marginal relief applied at each threshold. 4% Health & Education Cess on tax + surcharge.

**Gross income is solved via binary search** (256 iterations) — given a required post-tax monthly income, the calculator inverts the tax function to find the exact gross CTC needed.

---

## College SIP Methodology

Monthly SIP calculated as `total_cost_INR ÷ 230`, where 230 is the future-value factor for ₹1/month over 120 months at 1%/month (12% CAGR). This funds the full degree cost including living expenses.

| Destination | Assumed Cost | Monthly SIP / Child |
|---|---|---|
| 🇺🇸 USA | USD 80K/yr × 4 yrs (~₹2.79 Cr) | ₹1,21,000 |
| 🇬🇧 UK | GBP 37.5K/yr × 3 yrs (~₹1.29 Cr) | ₹56,000 |
| 🇨🇦 Canada | CAD 75K/yr × 4 yrs (~₹1.87 Cr) | ₹81,000 |
| 🇦🇺 Australia | AUD 80K/yr × 3 yrs (~₹1.35 Cr) | ₹59,000 |
| 🇸🇬 Singapore | SGD 38.8K/yr × 4 yrs (~₹1.03 Cr) | ₹45,000 |
| 🇮🇳 India (Top Tier) | ₹12.5 L/yr × 4 yrs (~₹50 L) | ₹22,000 |

Exchange rates as of Feb 2025.

---

## How to Use

1. Open `india-income-calculator.html` in any modern browser — no server or build step needed.
2. Use the floating **⚙️ Scenario Controls** panel (bottom-right corner) to adjust neighbourhood tier, number of kids, college destination, and travel budget.
3. The table updates instantly. Compare the **Required Gross Annual Income** row across cities to see how much more expensive Mumbai is vs Pune, etc.
4. Click "📊 New Tax Regime Slabs & Surcharge Brackets" (below the table) to see the full tax slab reference.

---

## Caveats & Methodology Notes

- All figures are **estimates** intended for comparative illustration, not financial planning.
- Home loan EMI assumes ~90% LTV on a premium 4BHK at prevailing market prices per city.
- School fees reflect top-tier IB/IGCSE schools; actual fees vary significantly.
- The "minimum" framing means no luxury excess — it's the floor for this lifestyle tier, not a comfortable buffer.
- No EPF / NPS deductions modelled (pure New Regime calculation).
- Values reflect **FY 2024–25** costs and tax rules.

---

## Credits

Original concept, research, and numbers: **[@deedydas](https://x.com/deedydas)**
Source: [https://x.com/deedydas/status/2025768860805476412](https://x.com/deedydas/status/2025768860805476412)
