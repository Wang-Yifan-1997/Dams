# Dams To-Do

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
