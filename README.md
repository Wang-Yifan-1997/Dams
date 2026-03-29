# Dams in Space — Research Agenda

---

## Data Assembly

### Core Datasets
| Dataset | Variables | Coverage |
|---|---|---|
| Dam registry | Location, year, size, type, function | National + Zhejiang detail |
| ASIE firm panel | Revenue, TFP, pollution, energy, industry code, location | 1998–2013 |
| Tax survey / VAT data | Revenue, employment, industry code, location | 2000–2020 |
| NBS county yearbooks | GDP, employment, investment | 1990–2020 |
| Nighttime lights | DMSP-OLS / VIIRS | 1992–present |
| Population census | Population, migration, education | 1982, 1990, 2000, 2010 |
| Water monitor stations | Water flow, water quality | 1990s–present |
| 国控断面 | Water quality monitoring | 2000s–present |
| River network | HydroSHEDS or national | National |
| Input-output tables | Water intensity, energy intensity by industry | Various years |
| China IO tables | Industry-level factor shares | 1997–2017 |

### Additional Data to Collect
- **Dam construction costs**: search infrastructure procurement databases, audit reports, local government gazettes, and World Bank project documents for a subsample. Target large dams post-2000 where disclosure is better
- **Government procurement data**: follow-up investments in dams (maintenance, upgrades, renewals) — connects to the optimal stopping / Rust (1987) angle
- **Electricity grid data**: provincial grid capacity and coverage by year — needed to test whether hydropower effects depend on grid connectivity
- **Road network data**: county-level road density by year — needed to test complementarity hypothesis
- **Flood event records**: historical flood damage data by county — needed to identify flood control channel
- **Resettlement records**: number of people displaced per dam, where available — needed for cost-side calculation
- **Fishery output data**: county-level freshwater fishery production — one measurable ecological cost
- **Upstream/downstream county assignment**: construct using river network + dam location (key spatial variable for mechanism identification)

### Descriptive Statistics to Produce
- Maps: spatial distribution of dams by type, size, and decade of construction; overlay with firm locations, river networks, county boundaries
- Figures: bar charts of dam construction by year, type, province; histogram of dam sizes; event study of firm outcomes around dam construction
- Tables: summary statistics (mean, median, min, max, std, p25, p75) for all key variables, separately for treated and control counties pre-treatment

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
- Does the treatment effect vary by county's pre-existing electricity access, water scarcity, or road connectivity? This tests the binding constraint hypothesis

### 1.2 Upstream/Downstream Spatial Design — Mechanism Identification

*This is the key identification innovation.*

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

**Design**: for each firm in ASIE, construct a 1/2/5 km buffer and assign treatment based on dam presence/construction within that buffer.

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

1. **Nighttime lights**: regress Δnighttime light (1998–1992) on Δdams (1998–1992), controlling for pre-period light levels
2. **Population**: regress Δpopulation (1990–1982) on Δdams (1990–1982) using census data
3. **Local projections**: for each horizon h = 0, 1, 2, ..., 10, regress Δoutcome(t, t+h) on Δdams(t). Plot the impulse response to trace out the dynamic effect of dam construction — tests whether effects are immediate or build over time, and whether they persist or decay

### 1.6 Dose-Response and Decreasing Returns

*Less causal but important for the welfare and optimality argument.*

- Regress log outcome on log(number of dams) or log(total dam capacity) — tests for decreasing returns in a flexible way
- Plot the marginal effect of an additional dam against the existing stock — identifies the point at which the marginal benefit falls below marginal cost
- This is the empirical input into the optimal stopping / welfare calculation

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

### 2.2 Indirect and Environmental Costs

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

### 2.3 Formal Welfare Model

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
- Map dam construction waves to political cycles: do dam approvals spike around Five-Year Plan transitions or leadership changes?

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

**Geographical / hydrological factors**:
- Rainfall variability in upstream catchment (CoV of annual rainfall) — tests the reliable water inflow hypothesis
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
- Number of other dams in the same watershed — tests system-level complementarity vs. congestion

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

- Using the dose-response estimates from Question 1.6 and the cost estimates from Question 2, characterize the marginal benefit and marginal cost of an additional dam as a function of the existing stock
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
2. What is the cost of dams — financial cost, other environmental costs, other indirect costs?
3. We might be able to find the follow-up investment for dams (from government procurement data). Then we will see when the government chooses to renew the dam, which somewhat speaks to the Rust (1987) optimal stopping / dynamic discrete choice literature, but placed in an environmental economics context, which is another way to make this paper popular
4. Dams are a form of government intervention. Why do we need government intervention? Why can the market not solve it? If the benefit-to-cost ratio is larger than 1, then let's say there is a big company that can charge people money and then build a dam. This market would exist if there were no market failure. Of course in the real world there are a lot of things going on, but in this hypothetical thought experiment, what is the market failure that prevents it from happening?
5. What is the optimal allocation of dams? What drives the optimality? There needs to be some clear tradeoff. This optimality is determined both by spatial optimality and time optimality
6. How does the government decide to build a dam? There must be a decision rule
7. Why do we stop building dams? Is it because benefits are smaller than costs, or political concerns, or other things?
8. Complementarity is currently a small section of Question 4, but is itself an interesting question that might need more attention. It would be good to frame the paper around complementarity, and therefore dam construction can be seen as a big push from one low equilibrium to a high equilibrium (poverty trap story and big push, which is commonly used to justify industrial policy)







# Dams To-Do (Original Draft Version)

## Data
- Dam registry: location, year, size, function (detailed information for one province, need to match with hydropower station with other data); follow-up investment (need to search online, not sure if this is useful)
- Manufacturing firm: revenue, TFP, patents (maybe less useful), pollution, energy consumption, firm entry, industry code
- County level: statistics yearbook, nighttime light, migration
- Water monitor station and water flow data (we need to have a think about what we can do with this data)
- Descriptive statistics: where are the dams, years, size, types
- We need both maps, figures (bar charts), and tables (mean, median, min, max, std, p25, p75)
- Replicate all the above analysis with Zhejiang Province and all other provinces

---

## Question 1: What is the role of dams?

### DiD Specification
1. From Zhejiang Province: flood control (43); electricity (109); water supply (303); irrigation (440); transportation (2); aquaculture (122); others (22)
2. County-level DiD: test the impact of dams on firm entry, revenue, TFP (labor productivity for ASIE), pollution for the building of new dams. Let's say we focus on a specific period of time (1998–2020) where most of the outcomes can be observed
3. Treatment = 1 if new dam > 0 after time t
4. Treatment = 1 if a certain type of new dam > 0 after time t. We might see bigger dams have a higher impact
5. Treatment = 1 if a certain size of new dam > 0 after time t
6. Treatment = 1 if a certain size & type of new dam > 0 after time t
7. Controlling the existing number of dams before the time window (before 1998), or controlling the existing dam size, or weighting the number/size of dams by the number of years since the dam was established to capture the fact that old dams might be less important since they are not functioning anymore
8. Interacting the existing number/size of dams before the time window with the treatment dummy. We might expect to see decreasing returns to scale of building dams

### Robustness
1. Replicate all the above analysis with Zhejiang Province and all other provinces (some regressions can only be run with Zhejiang Province)
2. Replicate all the above analysis with province-year fixed effects
3. Replicate all the above analysis with SDID, TWFE, and all other staggered DiD methods (some regressions can only be run with TWFE)

### Long-Difference Specification (where we can go a little bit further back)
1. Nighttime light data seems to exist from 1992. We can do: reg change of nighttime light (1998–1992) on the increase of dams (1998–1992)
2. Population census data seems to exist in 1982 (3rd) and 1990 (4th). Then reg change of population (1990–1982) on the increase of dams (1990–1982)
3. If we believe there is a lagged effect, then we can do a local projection approach: run the following regressions and collect the coefficients: reg change of outcome at t on the change of dams at t, at t-1, at t-2 etc. to see the impact

### Firm-Level Analysis
1. For each firm, if we know the location, calculate 1/2/5 km buffer. Every row is one firm
2. Define treatment = 1 if there are existing dams/new dams
3. Define treatment = 1 if there are existing dams/new dams for specific types or sizes
4. See if firm outcomes are affected by dam establishment
5. Ideally this would need a firm panel dataset (like ASIE, tax survey data etc.)
6. Question to think about: what do we learn from these firm-level regressions, other than serving as a robustness check?
7. It might be able to answer: do existing firms produce/pollute more, do large firms respond differently compared to small firms, do firms in different industries respond differently, what is the TFP response?
8. We can essentially replicate all the county-level analysis with the firm-level analysis, but we need to think about what additional insights we can get from here

### Less Causal but Helps Pin Down Parameters for Model
1. Instead of regressing outcome variables on a dam dummy, we just regress on the number of dams, size of dams, or a log-log relationship. We might see decreasing returns to dams. Then after a certain point, the benefit-to-cost ratio will be smaller than 1 if we also know what the cost is. This tells us what the optimal stopping point is. Otherwise it is less interesting if it is always optimal to build dams

---

## Question 2: What is the cost of dams?

1. Is there a way we can get the cost of dams, even for a subsample? Like for the large dams in recent years? If we try to speak to the benefit of dams without addressing the cost side, it would be a big miss
2. With maybe a few hundred or thousand dam construction costs, we do a prediction model (machine learning maybe) to predict all the remaining costs
3. Besides the monetary cost of dams, there is a more "economic" impact cost: environmental cost, resettlement and livelihood destruction, induced seismicity, sediment starvation, fishery destruction, loss of flood pulse, groundwater effects etc. Some of these costs we can try to estimate with a few regressions, and some we have to borrow from the existing literature
   - 3.1. We do a back-of-envelope calculation. We compare the cost of dams with the benefit of dams
   - 3.2. Alternatively, we put everything into a model and calculate the welfare
4. This exercise is much harder but I think it could distinguish our paper from many others if we can do a rigorous calculation of the cost and say something about welfare and optimality. This would involve a bit more data collection

---

## Question 3: How does the government decide to build a dam?

1. We need more background knowledge
2. One concern that people might raise is that if counties are going to grow faster, the government then decides to build a dam. This could explain why we see things going well after a dam is built, but that would be purely driven by dam endogeneity. Our DiD regression results could alleviate this concern (if we see parallel trends), but it would be much stronger if we have a good economic intuition and/or government policy as an IV

---

## Question 4: When do dams work and when do they fail?

1. We need to see every single dam's impact. Here are two ideas (of course these are initial ideas and we need to refine them later):
   - 1.1. For the firm-level regression, we assign the firm-level effect to each dam. This would be a dam-firm pair effect. Then aggregate these dam-firm pair effects to the dam level
   - 1.2. For each dam, we draw a 1/2/5 km buffer. Then for every dam, we draw 100 random points in the same province with 1/2/5 km buffers as a counterfactual control group. Use synthetic control or other matching methods to estimate the treatment effect of each dam
2. Then we will see the heterogeneous effects of dams. Next, we ask why some are more effective and some may backfire
3. Potential hypotheses:
   - 3.1. Geographical reasons: a dam requires sufficient and reliable water inflow to deliver consistent benefits
   - 3.2. Demand: an electricity-constrained area would benefit more from hydropower stations compared to a place with abundant electricity. Irrigation dams work well when agriculture is genuinely water-limited
   - 3.3. Complementarities: a single dam is not going to work. A dam with road connections and grid connections is going to work

---

## Other Random Thoughts — Not To-Do Tasks

1. What model should we have in mind?
2. What is the cost of dams — financial cost, other environmental costs, other indirect costs?
3. We might be able to find the follow-up investment for dams (from government procurement data). Then we will see when the government chooses to renew the dam, which somewhat speaks to the Rust (1987) optimal stopping / dynamic discrete choice literature, but placed in an environmental economics context, which is another way to make this paper popular
4. Dams are a form of government intervention. Why do we need government intervention? Why can the market not solve it? If the benefit-to-cost ratio is larger than 1, then let's say there is a big company that can charge people money and then build a dam. This market would exist if there were no market failure. Of course in the real world there are a lot of things going on, but in this hypothetical thought experiment, what is the market failure that prevents it from happening?
5. What is the optimal allocation of dams? What drives the optimality? There needs to be some clear tradeoff. This optimality is determined both by spatial optimality and time optimality
6. How does the government decide to build a dam? There must be a decision rule
7. Why do we stop building dams? Is it because benefits are smaller than costs, or political concerns, or other things?
8. Complementarity is currently a small section of Question 4, but is itself an interesting question that might need more attention. It would be good to frame the paper around complementarity, and therefore dam construction can be seen as a big push from one low equilibrium to a high equilibrium (poverty trap story and big push, which is commonly used to justify industrial policy)
