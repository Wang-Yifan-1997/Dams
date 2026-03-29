# Task Record — 2026-03-29

---

## Task 1 — Descriptive Statistics (Maps, Figures, Tables)

### 1.1 Maps
- Spatial distribution of dams by **type**, **size**, and **year of construction**
- Overlay dam locations with **river networks** and **county boundaries**
- Be innovative with map design — these are intended for paper inclusion and should meet **publication quality** standards (follow top-5 journal formatting conventions)

### 1.2 Figures
- Bar charts of dam construction by **year**, **type**, and **province**
- Histogram of **dam sizes**
- Same standards as above — publication quality

### 1.3 Summary Statistics Tables
Produce the following tables:

1. **Full sample**: summary statistics (`mean`, `median`, `min`, `max`, `std`, `p25`, `p75`, `count`) for all key variables
2. **Balance test**: the same statistics computed separately for **treated** and **control** counties, restricted to the **pre-treatment period** — this is to check balance before treatment

### 1.4 GitHub & Replication
- Push all code to GitHub with a clean, well-organized folder structure
- The repository should be **replication-ready**: clear README, commented code, and logical folder hierarchy so future RAs and collaborators can easily follow
- Consult an AI agent for best-practice proposals on structuring a large empirical project

---

## Task 2 — Baseline DiD: County-Level Estimation

**Sample**: all Chinese counties, 1998–2020

### 2.1 Outcome Variables
Run the analysis for each of the following outcomes:
- Firm entry (count of new firms)
- Firm revenue (log total revenue)
- TFP / labor productivity
- Pollution (SO₂, wastewater emissions, PM₂.₅)
- Energy consumption
- Nighttime light intensity
- County GDP

### 2.2 Treatment Definitions
Run the baseline specification separately for each treatment definition:
1. Any new dam built in county after year *t*
2. New dam of specific type (flood control / irrigation / hydropower / water supply) built after year *t*
3. New dam above/below median size built after year *t*
4. New dam of specific type **and** above/below median size built after year *t*

### 2.3 Controls
- Stock of existing dams before 1998 (count, total size, capacity-weighted age)
- Interaction of existing dam stock with treatment dummy (tests decreasing returns)
- County fixed effects, year fixed effects
- Province × year fixed effects (robustness)
- Pre-treatment county characteristics interacted with year trends

### 2.4 Estimators
- **TWFE** as baseline
- **Callaway–Sant'Anna** and **Sun–Abraham** for staggered timing robustness
- **SDID** as an additional check

### 2.5 Key Heterogeneity Tests
- Does the treatment effect vary by **dam type**? (mechanism test)
- Does the treatment effect decline with the **existing dam stock**? (decreasing returns test)
- Does the treatment effect vary by county's pre-existing **electricity access**, **water scarcity**, or **road connectivity**?

---

## Task 3 — Upstream/Downstream Spatial Design

*Spatial Regression Discontinuity for mechanism identification.*

### 3.1 Sample Construction
- For each dam, classify counties as **upstream** or **downstream** using river network data
- Compare upstream vs. downstream counties of the **same dam**, before and after construction

### 3.2 Predicted Effects by Channel

| Channel | Upstream effect | Downstream effect | Asymmetry |
|---|---|---|---|
| Flood control | None | Positive | Yes |
| Water supply / irrigation | None | Positive | Yes |
| Hydropower / electricity | Symmetric | Symmetric | No |
| Pollution | Negative (less dilution) | Positive (more dilution) | Yes, opposite sign |

### 3.3 Key Tests to Run
- **Flood control dams**: downstream firm entry and investment should increase; upstream should not
- **Hydropower dams**: energy-intensive firm output should increase symmetrically on both sides
- **Water supply dams**: water-intensive firm output should increase downstream only
- **Pollution**: test for differential pollution intensity upstream vs. downstream

> The upstream/downstream asymmetry is the core falsification test. Symmetric effects for water channels, or asymmetric effects for electricity, would indicate something is wrong with the mechanism. Please flag any unexpected patterns immediately.

---

## Task 4 — Industry Heterogeneity

*Pinning down the mechanism using industry-level water and energy intensity from IO tables.*

### 4.1 Key Regressions
- Revenue growth in **water-intensive vs. non-water-intensive** industries downstream of **irrigation dams**
- TFP growth in **energy-intensive vs. non-energy-intensive** industries near **hydropower dams**
- Pollution growth in **polluting industries** downstream of **water supply dams**

### 4.2 What This Should Show
Effects should not only be spatially concentrated (Task 3) but also industrially concentrated in exactly the way theory predicts. The interaction of dam type × industry intensity is the micro-foundation of the mechanism — please present these results clearly and systematically.

---

## Task 5 — Firm-Level Analysis

### 5.1 Sample Construction
- For each firm in ASIE, construct a **1 km / 2 km / 5 km buffer** around the firm's location
- Assign treatment based on dam presence or new construction **within that buffer**

### 5.2 Outcome Variables
- Firm revenue, TFP, pollution emissions, energy consumption, employment, exit/survival

### 5.3 Key Questions
- Do **incumbent firms** increase output after a nearby dam is built?
- Do **large and small firms** respond differently?
- Do firms in **water-intensive vs. non-water-intensive industries** respond differently?
- Does **TFP** increase (genuine productivity gain) or just output (scale effect)?
- Do firms **pollute more** after water supply improves? (dilution/enforcement cost mechanism)

---

## Task 6 — Long-Difference Specifications & Local Projections

### 6.1 Long Differences
1. Regress Δnighttime light (1998–1992) on Δdams (1998–1992)
2. Regress Δpopulation (1990–1982) on Δdams (1990–1982) using census data

### 6.2 Local Projections (Impulse Response)
- For each horizon *h* = 0, 1, 2, ..., 10, regress Δoutcome(*t*, *t*+*h*) on Δdams(*t*)
- Plot the **impulse response function** to trace out the dynamic effect of dam construction
- Key questions: Are effects immediate or do they build over time? Do they persist or decay?

---

## Task 7 — Dose-Response and Decreasing Returns

- Regress log outcome on log(number of dams) or log(total dam capacity)
- Plot the **marginal effect** of an additional dam against the existing stock
- Goal: test for decreasing returns in a flexible, non-parametric way — this feeds directly into the welfare and optimality argument

---

*Last updated: 2026-03-29*
