# Dams in Space — Research Agenda

---

## Data Assembly

### Core Datasets
| Dataset | Variables |
|---|---|
| Dam registry (6 province + Zhejiang detail) | Location, year, size, type, function |
| firm panel (ASIE and polluting firms) | Revenue, TFP, pollution, energy, industry code, location |
| Tax survey | Revenue, employment, industry code, location |
| county yearbooks | GDP, employment, investment |
| Nighttime lights | DMSP-OLS / VIIRS |
| Population census | Population, migration, education |
| Water monitor stations, 国控断面 etc | Water flow, water quality |
| River network | HydroSHEDS or national |
| Flood event data | Global Flood Data |
| Input-output tables | Water intensity, energy intensity by industry |

### Potential Additional Data to Collect
- **Dam construction costs**: search infrastructure procurement databases, audit reports, local government report, and World Bank project documents for a subsample. Target large dams post-2000 where disclosure is better
- **Government procurement data**: follow-up investments in dams (maintenance, upgrades, renewals)
- **Electricity grid data and Road network data**: provincial grid capacity, county-level road density — needed for infrastructure complementarity story
- **Upstream/downstream county assignment**: construct using river network + dam location (key spatial variable for mechanism identification)

### Descriptive Statistics to Produce (Zhenglin)
- Maps: spatial distribution of dams by type, size, and year of construction; overlay with river networks and county boundaries
- Figures: bar charts of dam construction by year, type, province; histogram of dam sizes
- Tables: summary statistics (mean, median, min, max, std, p25, p75) for all key variables, separately for treated and control counties pre-treatment (Balance Test)

---

## Question 1: What is the Role of Dams?

*Core identification question. Clean causal estimates of dam effects on economic activity, disaggregated by dam type and mechanism.*

### 1.1 Baseline DiD — County Level

**Sample**: all Chinese counties, 1998–2020

**Outcome variables**:
- Firm entry (count of new firms)
- Firm revenue (log total revenue)
- TFP / labor productivity
- Pollution (SO₂, wastewater emissions, PM2.5)
- Energy consumption
- Nighttime light intensity
- County GDP

**Treatment definitions** (run separately):
1. Treatment = 1 if any new dam built in county after year t
2. Treatment = 1 if new dam of specific type (flood control / irrigation / hydropower / water supply) built after year t
3. Treatment = 1 if new dam above/below median size built after year t
4. Treatment = 1 if new dam of specific type AND above/below median size after year t

**Controls**:
- Stock of existing dams before 1998 (count, total size, capacity-weighted age)
- Interaction of existing dam stock with treatment dummy (tests decreasing returns)
- County fixed effects, year fixed effects
- Province × year fixed effects (robustness)
- Pre-treatment county characteristics interacted with year trends

**Estimators**: TWFE as baseline; Callaway–Sant'Anna and Sun–Abraham for staggered timing robustness; SDID as additional check

**Key heterogeneity tests**:
- Does the treatment effect vary by dam type? This is the mechanism test
- Does the treatment effect decline with existing dam stock? This tests decreasing returns
- Does the treatment effect vary by county's pre-existing electricity access, water scarcity, or road connectivity? 

### 1.2 Upstream/Downstream Spatial Design — Mechanism Identification

*Spatial Regression Discontinuity.*

**Design**: for each dam, classify counties as upstream or downstream using river network data. Compare upstream vs. downstream counties of the same dam, before and after construction.

**Predictions by channel**:
| Channel | Upstream effect | Downstream effect | Asymmetry |
|---|---|---|---|
| Flood control | None | Positive | Yes |
| Water supply / irrigation | None | Positive | Yes |
| Hydropower / electricity | Symmetric | Symmetric | No |
| Pollution | Negative (less dilution) | Positive (more dilution) | Yes, opposite sign |

**Key tests**:
- Flood control dams: downstream firm entry and investment should increase; upstream should not
- Hydropower dams: energy-intensive firm output should increase symmetrically on both sides
- Water supply dams: water-intensive firm output should increase downstream only
- Pollution: upstream counties should see higher pollution intensity; downstream lower — or vice versa depending on dilution effect

**Why this matters**: the upstream/downstream asymmetry is the cleanest possible falsification test. If you find symmetric effects for water channels, the mechanism is wrong. If you find asymmetric effects for electricity, something else is going on. This design rules out most alternative explanations.

### 1.3 Industry Heterogeneity — Pinning Down the Mechanism

**Design**: interact dam type with industry-level water intensity and energy intensity (from IO tables). Test whether downstream gains are concentrated in water-intensive industries for water supply dams, and energy-intensive industries for hydropower dams.

**Key regressions**:
- Revenue growth in water-intensive vs. non-water-intensive industries downstream of irrigation dams
- TFP growth in energy-intensive vs. non-energy-intensive industries near hydropower dams
- Pollution growth in polluting industries downstream of water supply dams (the core environmental externality)

**What this adds**: this is the micro-foundation of the mechanism. We want to see that the effects are not just spatially concentrated but industrially concentrated in exactly the way the theory predicts.

### 1.4 Firm-Level Analysis

**Design**: for each firm in ASIE, construct a 1/2/5 km buffer and assign treatment based on dam presence/new construction within that buffer.

**Outcome variables**: firm revenue, TFP, pollution emissions, energy consumption, employment, exit/survival

**Key questions this can answer**:
- Do incumbent firms increase output after a nearby dam is built?
- Do large and small firms respond differently?
- Do firms in different industries respond differently (water-intensive vs. not)?
- Does TFP increase (genuine productivity gain) or just output (scale effect)?
- Do firms pollute more after water supply improves (the dilution/enforcement cost mechanism)?

**What firm-level adds over county-level**: identifies whether effects are driven by incumbent firm expansion or new firm entry; tests industry heterogeneity at finer resolution; allows firm fixed effects that absorb all time-invariant firm characteristics

### 1.5 Long-Difference Specifications

*Extends the analysis further back in time using coarser data.*

1. **Nighttime lights**: regress Δnighttime light (1998–1992) on Δdams (1998–1992)
2. **Population**: regress Δpopulation (1990–1982) on Δdams (1990–1982) using census data
3. **Local projections**: for each horizon h = 0, 1, 2, ..., 10, regress Δoutcome(t, t+h) on Δdams(t). Plot the impulse response to trace out the dynamic effect of dam construction — tests whether effects are immediate or build over time, and whether they persist or decay

### 1.6 Dose-Response and Decreasing Returns

*Less causal but important for the welfare and optimality argument.*

- Regress log outcome on log(number of dams) or log(total dam capacity) — tests for decreasing returns in a flexible way
- Plot the marginal effect of an additional dam against the existing stock

---

## Question 2: What is the Cost of Dams?

*Without the cost side, the paper cannot speak to welfare or optimality. This is what distinguishes it from the existing literature.*

### 2.1 Direct Financial Costs

**Data collection strategy**:
- Search infrastructure procurement databases, National Audit Office reports, local government budget documents, and World Bank/ADB project documents
- Target large dams (capacity > X MW or reservoir > Y km²) post-2000 where disclosure requirements are stricter
- Aim for a subsample of a few hundred dams with documented costs

**Prediction model for remaining dams**:
- Train a machine learning model (XGBoost or LASSO) on the subsample with cost as outcome and dam characteristics (size, type, location, year, geology, remoteness) as features
- Predict construction costs for all dams in the registry
- Use predicted costs as inputs into the cost-benefit calculation

**Key variables to collect**:
- Total project cost (engineering + equipment + civil works)
- Resettlement cost (often reported separately or omitted)
- Financing cost (interest during construction)
- Time to completion (to calculate cost overruns)

### 2.2 Indirect and Environmental Costs (need more thoughts)

**Costs we can estimate empirically**:
- Resettlement: number of people displaced × estimated compensation shortfall (from survey literature on resettlement welfare)
- Fishery destruction: regress county-level freshwater fishery output on upstream dam construction — estimate the fishery loss
- Agricultural productivity loss downstream over time: test whether the short-run positive effects on downstream agriculture decay over decades (sediment starvation mechanism)
- Upstream flooding of agricultural land: use satellite land cover data to measure reservoir inundation area

**Costs we borrow from the literature**:
- Sediment starvation and delta land loss: use estimates from hydrology and geomorphology literature
- Induced seismicity: use existing estimates of earthquake risk near large reservoirs
- Health costs of pollution: use VSL-based estimates from the Chinese health economics literature (e.g., He, Fan, Zhou)

**Back-of-envelope welfare calculation**:
- For each dam type, sum estimated benefits (from Question 1 regressions) and estimated costs (financial + environmental)
- Express as benefit-cost ratio
- Identify which dam types and locations have ratios above and below 1
- This directly answers: was it socially optimal to build these dams?

### 2.3 Formal Welfare Model (need more thoughts)

*The quantitative payoff of the paper.*

**Structure**:
- Three benefit channels: flood protection value, water supply value, electricity value
- Each calibrated from the empirical estimates in Question 1
- One cost channel: pollution externality (valued using marginal damage of emissions on local health)
- Financial cost channel: predicted construction cost from 2.1
- Environmental cost channel: fishery, sediment, resettlement (from 2.2)

**Output**:
- Net welfare by dam type and geography
- Aggregate welfare effect of China's dam program
- Counterfactual: what if China had invested the same capital in alternative infrastructure (roads, grid, solar)?

---

## Question 3: How Does the Government Decide to Build a Dam?

*Needed both as background and as the basis for an IV or quasi-experimental design to address endogeneity.*

### 3.1 Descriptive Political Economy

- Document the institutional process: who proposes, approves, and finances dam construction in China? How has this changed across planning eras (Maoist, reform, post-2000)?
- Identify the key decision-makers: central vs. provincial vs. county government; Ministry of Water Resources vs. National Development and Reform Commission; state-owned construction enterprises

### 3.2 Regression Analysis of Dam Placement

- What predicts dam construction? Regress dam construction on: river gradient, catchment area, distance to existing grid, population density, pre-existing flood damage, agricultural water scarcity, provincial fiscal capacity
- This serves two purposes: understanding the political economy, and identifying the variables that need to be controlled for or used as instruments

### 3.3 Identification Strategy for Endogeneity

*The parallel trends assumption in the DiD is the first line of defense, but an IV would be much stronger.*

**Candidate instruments**:
- **River gradient × national electricity demand shocks**: steep rivers are good hydropower sites; interacting with national demand shocks generates plausibly exogenous variation in hydropower dam timing
- **Geological suitability**: areas with the right topography for dam construction (narrow gorges, large upstream catchment) are more likely to get dams; this is predetermined and uncorrelated with future economic trends conditional on controls
- **Historical dam plans**: China's various water resource plans (e.g., the 12 hydropower bases identified in national plans) pre-committed to dam construction in specific locations before local economic conditions were observed
- **Upstream rainfall shocks**: in a staggered design, counties downstream of rivers experiencing unusual rainfall events may see accelerated dam construction for flood control reasons unrelated to local economic growth

**Falsification tests**:
- Show no pre-trends in outcome variables before dam construction
- Show that dam construction is not predicted by pre-treatment growth in outcomes
- Show that effects are stronger for dams whose construction timing was determined by centrally planned decisions rather than local requests

---

## Question 4: When Do Dams Work and When Do They Fail?

*The heterogeneity question. Important for policy and for understanding the mechanism.*

### 4.1 Estimating Individual Dam Effects

**Method 1 — Firm-to-dam aggregation**:
- Run firm-level regressions assigning each firm to its nearest dam(s)
- Aggregate firm-level treatment effects to the dam level (weighted by firm size or distance)
- Produces a distribution of dam-level effects

**Method 2 — Synthetic control for each dam**:
- For each dam, draw a buffer zone (1/2/5 km)
- Draw 100 random placebo locations in the same province as counterfactual
- Use synthetic control or matching to estimate the treated vs. counterfactual outcome path for each dam
- Produces a dam-level treatment effect with a data-driven counterfactual

**Output**: a distribution of dam effects — what fraction of dams have positive effects, what fraction negative, what is the variance?

### 4.2 Why Do Some Dams Work? Heterogeneity Analysis

Once you have dam-level effects, regress them on dam and location characteristics:

**Geographical / hydrological factors** (need more thought):
- Rainfall variability in upstream catchment — tests the reliable water inflow hypothesis
- River sediment load — tests whether high-sediment rivers produce faster-degrading dams
- Distance to coast / delta — tests whether downstream ecological costs are higher for dams closer to sensitive coastal ecosystems

**Demand-side factors**:
- Pre-dam electricity access in county (share of firms reporting power outages) — tests binding constraint hypothesis for hydropower
- Pre-dam irrigation coverage — tests binding constraint for irrigation dams
- Pre-dam water scarcity index — tests whether water supply dams work better in dry areas

**Complementarity factors**:
- Road density at time of dam construction — tests road complementarity
- Grid connectivity at time of dam construction — tests grid complementarity for hydropower
- Distance to nearest city — tests market access complementarity
- Number of other dams in the same watershed — tests complementarity vs. congestion

**Dam characteristics**:
- Size (larger dams may have larger effects but also larger costs)
- Age (do effects decay as dams silt up?)
- Type (the core heterogeneity of the paper)
- Multi-purpose vs. single-purpose (do multi-purpose dams deliver more?)

### 4.3 Complementarity as a Standalone Contribution

*This could be developed into a separate section or even a separate paper. It connects to the big push / poverty trap literature.*

**Core argument**: a dam alone may not be sufficient to shift a local economy. But a dam combined with grid connection, road access, and functioning markets may constitute a big push that moves the economy from a low to a high equilibrium. This is the Rosenstein-Rodan (1943) / Murphy-Shleifer-Vishny (1989) mechanism applied to infrastructure.

**Empirical test**:
- Regress dam treatment effect on an index of complementary infrastructure at the time of construction
- Test for non-linearities: does the dam effect jump discontinuously once complementary infrastructure crosses a threshold?
- Look for spatial clustering of successful dams — if complementarity matters, successful dams should cluster in areas where multiple infrastructure investments arrived around the same time

**Why this might be a way to frame contribution**: the big push / coordination failure framing provides a clean theoretical structure, connects to a classic literature, and has clear policy implications — the returns to dam investment depend on the broader infrastructure bundle, which means sequencing and coordination of public investment matters.

### 4.4 Optimal Stopping — When Should the Government Stop Building?

*Connects the empirical findings to the Rust (1987) literature and provides a normative conclusion.*

- Using the estimates from Question 1.6 and the cost estimates from Question 2, characterize the marginal benefit and marginal cost of an additional dam as a function of the existing stock
- Identify the optimal stopping point: the level of dam investment at which marginal benefit equals marginal cost
- Compare to China's actual investment trajectory: has China overbuilt or underbuilt relative to the optimum?
- Use government procurement data on dam renewals and upgrades to test whether renewal decisions are consistent with optimal stopping behavior

---

## Robustness and Sensitivity

- Replicate all county-level analysis for Zhejiang Province alone (where dam registry is most detailed) and for all other provinces separately
- Replicate with province × year fixed effects throughout
- Replicate with TWFE, Callaway–Sant'Anna, Sun–Abraham, and SDID estimators
- Test sensitivity of welfare calculations to alternative valuations of environmental costs (low, central, high estimates)
- Test sensitivity to the machine learning cost prediction model (alternative algorithms, different training samples)
- Placebo tests: assign fake dam locations or fake construction dates; confirm no effects

---

## Paper Structure (Suggested)

1. Introduction
2. Background: dams in China, institutional context, political economy of dam construction
3. Data and descriptive statistics
4. Empirical strategy: DiD design, upstream/downstream spatial design, identification
5. Results: county-level effects by dam type
6. Mechanism: industry heterogeneity, upstream/downstream asymmetry, water monitor station evidence
7. Costs: financial costs, environmental costs, back-of-envelope welfare
8. Heterogeneity: when do dams work? geographical, demand-side, complementarity
9. Welfare model and optimality
10. Conclusion

---

## Other Random Thoughts — Not To-Do Tasks

1. What model should we have in mind?
2. The cost of dams — financial cost, other environmental costs, other indirect costs?
3. We might be able to find the follow-up investment for dams (from government procurement data). Then we will see when the government chooses to renew the dam, which somewhat speaks to the Rust (1987) optimal stopping / dynamic discrete choice literature, but placed in an environmental economics context, which is another way to make this paper popular
4. Dams are a form of government intervention. Why do we need government intervention? Why can the market not solve it? If the benefit-to-cost ratio is larger than 1, then let's say there is a big company that can charge people money and then build a dam. This market would exist if there were no market failure. Of course in the real world there are a lot of things going on, but in this hypothetical thought experiment, what is the market failure that prevents it from happening?
5. What is the optimal allocation of dams? What drives the optimality? There needs to be some clear tradeoff. This optimality is determined both by spatial optimality and time optimality
6. How does the government decide to build a dam? There must be a decision rule
7. Why do we stop building dams? Is it because benefits are smaller than costs, or political concerns, or other things?
